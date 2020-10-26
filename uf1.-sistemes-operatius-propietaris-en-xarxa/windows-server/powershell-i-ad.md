# Powershell i AD

PowerShell pot interactuar amb qualsevol subsistema de Windows. És particularment útil quan s'utilitza per consultar Active Directory. A continuació trobaràs una selecció de scripts interessants, però, abans de veure'ls, hauràs de comprovar que el mòdul d'Active Directory estigui habilitat al shell. La forma més senzilla és executar aquestes comandes al **controlador de domini**, però si això no és possible, també pots afegir el paper al teu sistema, tot i que haurà de formar part de l'domini des del qual es vol extreure la informació.

```text
Add-WindowsFeature RSAT-AD -PowerShell
```

Un cop s'ha completat la instal·lació, pots fer servir l'intèrpret d'ordres per recopilar informació i modificar el domini d'Active Directory. Fes-ho amb compte si estàs treballant en un entorn de producció.

Com la llista d'ordres és bastant gran, hem agrupat per categories els cmdlets que considerem més útils. Sempre pots executar la comanda **get-help** per obtenir més informació sobre aquests **cmdlets**.



## **Consulta d'usuaris**

Consultar els comptes d'usuari és molt senzill en PowerShell. Els comandos  de la taula següent expliquen com buscar usuaris, identificar aquells amb contrasenyes no compatibles, bloquejats, deshabilitats, etc.

| comando PowerShell | Què fa | categoria |
| :--- | :--- | :--- |
| _`Get-ADUser -Filter * -Properties * | where { $_.whenCreated -ge $week } | select Name,whenCreated | Sort Name`_ | Usuaris creats en l'última setmana, ordenats per nom | USERS |
| _`Get-ADUser -Filter * -Properties PasswordNeverExpires | where { $_.PasswordNeverExpires -eq $true } | select Name | sort Name`_ | Usuaris amb contrasenya configurada "sense caducitat", ordenats per nom | USERS |
| _`Get-ADUser -Filter “Enabled -eq ‘$false'” | Select Name, UserPrincipalName | Sort name`_ | Usuaris amb comptes INACTIVES, mostra els noms i els FQN \(noms certificats\), ordenats per nom | USERS |
| _`Search-ADAccount -AccountDisabled -UsersOnly | FT Name,ObjectClass -A`_ | Usuaris amb comptes deshabilitades, mostra els noms i els FQN, ordenats per nom | USERS |
| _`Search-ADAccount -LockedOut | Format-Table name,lastlogondate, lockedout, objectclass, passwordexpired, passwordneverexpires`_ | Troba usuaris amb el compte bloquejat | USERS |
| _`Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 -UsersOnly |Sort-Object | FT Name,ObjectClass -A`_ | Troba els comptes d'usuaris que no han estat utilitzades durant 90 dies | USERS |
| _``Get-ADuser -Filter {name -like “*”} -properties *|select @{N=”Account”;E={$_.name}},@{N=”Name”;E={$_.givenname}},@{N=”LastName”;E={$_.surname}},@{N=”Mail”;E={$_.mail}},@{N=”AccountEnabled”;E={$_.enabled}},@{N=”MemberOf”;E={(Get-ADPrincipalGroupMembership $_).name -join (“`r`n”,”,,,,,”)}} | Sort-Object “Account” | FT -AutoSize``_ | Grups de pertinença per a tots els usuaris. Ordena les dades en forma de taula. Usa Export-CSV per tornar un fitxer CSV. | USERS |
| `Get-ADUser -Filter * -Properties LastLogonDate | ? { $_.LastLogonDate -eq $null } | Select name,samaccountname` | Troba els usuaris que no han iniciat mai la sessió | USERS |



## **Consulta de grups**

La pertinença a un grup d'Active Directory té importants repercussions en la seguretat. Per aquesta raó, és molt útil poder determinar ràpidament la presència en grups per a cada usuari, per tal de garantir el compliment de les polítiques de seguretat internes.

| comando | Què fa | categoria |
| :--- | :--- | :--- |
| `get-adgroup -filter * -Properties GroupCategory | Seleccioneu el nom, la categoria del grup | FT -A` | Tots els grups presents en Active Directory | GROUPS |
| `Get-ADGroupMember -identity “Administradors” -recursiu | seleccioneu el nom` | Tots els usuaris de el grup Administradors de domini \(Domain Administrators\) | GROUPS |
| `Get-ADPrincipalGroupMembership -identity Turbogeek | Sort-object | FT -property name, samaccountname -AutoSize` | Troba els grups als quals pertany un usuari | GROUPS |
| Get-ADGroupMember -Administradors de dominis d’identitat ”-Recursius \| % {Get-ADUser -Identity $ \_. DistinctionName -Properties activat \| ? {$ \_. Activat -eq $ false}} \| Seleccioneu Nom distingit, Activat | Troba els usuaris deshabilitats en el grup Administradors de domini | GRUPS |

## **Consulta de la infraestructura d'Active Directory**

Aquestes ordres proporcionen informació sobre la infraestructura.

| comando | Què fa | categoria |
| :--- | :--- | :--- |
| Get-ADDomainController -Filter \* \| Nom de la taula de formats, domini, bosc, lloc, adreça ipv4, sistema operatiu | Troba el controlador de domini de l'domini | DC |
| Get-ADDomainController -Filter {IsGlobalCatalog -eq $ true} \| Nom de l'objecte de selecció, adreça ipv4, isglobalcatalog, operatingsystem \| FT -A | Troba el servidor de catàleg global en el domini, útil si tens més d'un controlador de domini | DC |
| Get-ADDomainController -Filter {IsReadOnly -eq $ true} | Troba el controlador de domini de només lectura si existeix en la infraestructura \(Branch Servers\) | DC |
| Get-ADComputer -Filter 'Name -like "The-Solving-N1 \*"' -Properties canonicalName, CN, created, IPv4Address, objectclass, OperatingSystem, OperatingSystemServicePack \| FT -A | Troba l'ordinador en el domini com "The-Solving-N1", mostra informació útil en forma de taula | DC |
| Get-ADForest \| Select-Object -ExpandProperty ForestMode | Obté el nivell de bosc de l'Active Directory | DC |
| Get-ADDomain \| Select-Object -ExpandPropertymode de domini | Obté el nivell de domini d'Active Directory | DC |
| Get-ADReplicationConnection -Filter {AutoGenerated -eq $ true} | Obté detalls sobre la resposta de l'domini. Les dades es tornaran només si hi ha més d'un controlador de domini present. | DC |
| $ datecutoff = \(Get-Date\) Get-ADComputer -Filter {LastLogonTimestamp -lt $ datecutoff} -Properties Name, LastLogonTimeStamp \| Seleccioneu Nom, @ {N = 'LastLogonTimeStamp'; E = {\[DataTime\] :: FromFileTime \($ \_. LastLogonTimeStamp\)}} | Executa aquest script des PowerShell ISE. Configura el $ datecutoff i això indicarà l'horari de l'últim inici de sessió d'un ordinador. | DC |

## **Modifica Active Directory**

També pots utilitzar PowerShell per modificar el domini d'Active Directory. Usa aquestes comandes amb precaució, especialment si ets principiant en PowerShell.

| comando | Què fa | categoria |
| :--- | :--- | :--- |
| Disable-ADaccount -identity The-Solving | Deshabilita el compte The-Solving | USUARIS |
| Enable-ADaccount -identity The-Solving | Habilita el compte The-Solving | USUARIS |
| Set-ADAccountExpiriation -Identity The-Solving -datetime "07/01/2019" | Configura el compte The-Solving perquè expiri el 7 de gener de 2019 | USUARIS |
| Clear-ADAccountExpiration -identity The-Solving | Desactiva la data de caducitat del compte | USUARIS |
| Set-ADAccountPassword -identity The-Solving -reset -newpassword \(Convertto-Securestring -asplaintext "Passw0rd123!" -Force\) | Canvia la contrasenya d'usuari xifrant la transmissió - essencial | USUARIS |
| Unlock-ADAccount -identity The-Solving | Desbloqueja el compte The-Solving | USUARIS |
| New-AdGroup -Name "Test Users" -SamAccountName TestUser -GroupCategory Security -GroupScope Global -displayname 'Test Users' -Path "OU = Groups, OU = Resources, DC = TEST, DC = UK -Description" All Test Users " | Crea un grup de seguretat anomenat Test Users a OU Groups&gt; Resources | GRUPS |
| Set-ADGroup -Identity 'Test Users' -groupcategory Distribution -groupscope Universal -Managedby 'TurboGeek' | Modifica el grup Test Users i el converteix en un grup universal de distribució, administrat per tu | GRUPS |
| search-adaccount -lockedout \| unlock-adaccount -passthru -confirm | Troba i desbloqueja tots els comptes "locked" en Active Directory. Una solució senzilla i eficaç per desbloquejar un grup. | USUARIS |
|  |  |  |

