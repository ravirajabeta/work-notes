**<ins>Collection Query Rule to add the list of machines :</ins>**

select * from SMS_R_System where SMS_R_System.Name in ("5CG5270G4B",	"5CG4453ZNS",	"5CG533795N",	"5CG6255913",	"5CG4454HM3",	"5CG4483FMP",	"CNU431BKFX",	"5CG529400Y",	"5CG5337920",	"5CG52943DG",	"5CG4523D78",	"CNU25198XG",	"5CG4503XHG",	"5CG529410Z",	"5CG625580G",	"5CG4453Z90",	"5CG529424T",	"5CG7213SNK",	"5CG6255GVR",	"5CG5342Z0M",	"5CG52940JG",	"5CG5070707")

**<ins>SQL Query to get the Installed Software Version on SCCM Clients:</ins>**

select distinct vrs.Name0,vrs.Operating_System_Name_and0,vrs.Resource_Domain_OR_Workgr0,vga.DisplayName0,vga.Version0 
from v_R_System as vrs inner join v_GS_ADD_REMOVE_PROGRAMS as vga on vrs.ResourceID = vga.ResourceID 
where vga.DisplayName0 like 'CrowdStrike Windows Sensor' 

select distinct vrs.Name0,vrs.Operating_System_Name_and0,vrs.Resource_Domain_OR_Workgr0,vga.DisplayName0,vga.Version0 
from v_R_System as vrs inner join v_GS_ADD_REMOVE_PROGRAMS as vga on vrs.ResourceID = vga.ResourceID 
where vga.DisplayName0 like 'Citrix%'
and vrs.Resource_Domain_OR_Workgr0 like 'RETAIL'
and vrs.Name0 like 'S00962REG1'
/* and vrs.Name0 in ('S00770REG2',  'S00770REG4')  

**<ins>SQL Query to list all the members of a Collection:</ins>**

select * from v_Collection where Name like 'All Desktop and Server Clients' è Get the Collection ID
select * from v_Collection where CollectionID like 'SMSDM003' è Get the Collection Name

select * from v_FullCollectionMembership where CollectionID like 'AA1005A8' è Get the Collection Members list

select * from v_Collection where Name like 'All Desktop and Server Clients'
Select distinct fcm.CollectionID,col.Name,col.MemberCount,fcm.Name,fcm.Domain,fcm.SiteCode from v_FullCollectionMembership as fcm inner join v_Collection as col on fcm.CollectionID = col.CollectionID where col.CollectionID like 'AA1005A8'

Select distinct fcm.CollectionID,col.Name,fcm.Name,fcm.Domain,vrs.Operating_System_Name_and0 as 'Operating System'
from v_FullCollectionMembership as fcm 
inner join v_Collection as col on fcm.CollectionID = col.CollectionID 
inner join v_R_System as vrs on fcm.ResourceID = vrs.ResourceID 
where col.CollectionID like 'AA1002CC'

Select distinct fcm.CollectionID,col.Name as 'Collection Name',col.MemberCount,fcm.Name,fcm.Domain,fcm.SiteCode from v_FullCollectionMembership as fcm 
inner join v_Collection as col on fcm.CollectionID = col.CollectionID where col.CollectionID in ('AA10019C',    'AA10019E',    'AA10019F',    'AA1001A0',    'AA1001A2',    'AA1001A3',    'AA1001A4',    'AA1001A5',    'AA1001A6',    'AA1001A7',    'AA1001A8',    'AA1001A9',    'AA1001AA',    'AA1001AB',    'AA1001AC',    'AA1001AD',    'AA1001AE',    'AA1001AF')

Select distinct fcm.CollectionID,col.Name,col.MemberCount,fcm.Name,fcm.Domain,fcm.SiteCode from v_FullCollectionMembership as fcm inner join v_Collection as col on fcm.CollectionID =col.CollectionID where col.CollectionID like'AA1002E9'
Select distinct fcm.CollectionID,col.Name,col.MemberCount,fcm.Name,fcm.Domain,fcm.SiteCode from v_FullCollectionMembership as fcm inner join v_Collection as col on fcm.CollectionID =col.CollectionID where col.CollectionID like'AA1002EA'
Select distinct fcm.CollectionID,col.Name,col.MemberCount,fcm.Name,fcm.Domain,fcm.SiteCode from v_FullCollectionMembership as fcm inner join v_Collection as col on fcm.CollectionID =col.CollectionID where col.CollectionID like'AA1002EB'

**<ins>SQL Query to get all the Applications assigned to specific collection with user experience settings</ins>**
 
select AA.ApplicationName,coll.Name,coll.CollectionID as 'Collection Name',
case when NotifyUser=1 then 'Yes' Else 'No' End as 'Notify User',
case when UserUIExperience=1 then 'Yes' Else 'No' End as 'UserUIExperience',
case when AssignmentAction=2 then 'Install' Else 'Uninstall' End as 'Action',
case when ads.DeploymentIntent=2 then 'Avilable' Else 'Required' End as 'Purpose',
AA.CreationTime,AA.LastModificationTime,AA.LastModifiedBy from v_ApplicationAssignment AA,v_Collection coll ,vAppDeploymentStatus ADS
where (AA.CollectionID=coll.CollectionID) and (AA.AssignmentID=ADS.AssignmentID) and
/*coll.CollectionID in ('AA1003F7',    'AA100608',    'SMSDM003') */
coll.CollectionID like 'AA100608'
and (AA.AssignmentName not like '%Software Update%' and AA.AssignmentName not like '%Endpoint Protection%')

**<ins>SQL Query to get Packages Assigned to specific collection:</ins>**

select * from v_Collection where Name like 'NewRelic(POC) All Locations' è Get the Collection ID
select * from v_Collection where CollectionID like 'AA1005FA' è Get the Collection Name

select adv.AdvertisementName [Advertisement Name],adv.packageid [Package ID],
adv.ProgramName,adv.ExpirationTime,case when AssignedScheduleEnabled=0 then 'Available' Else 'Required' End as 'Purpose'
from v_Advertisement adv,v_Collection coll
where adv.CollectionID=coll.CollectionID
and coll.CollectionID = 'AA1005FA'
 
**<ins>SQL Query to get Application Deployment Status against a collection :</ins>**

SELECT distinct
CollectionName,
vrs.Name0 [Computer Name],vrs.Client0 [SCCM Client] ,vrs.Client_Version0 [SCCM Client version],vgos.Caption0 [OS],vrs.User_Name0 [User Name],
IIf([EnforcementState]=1001,'Installation Success',
IIf([EnforcementState]>=1000 And [EnforcementState]<2000 And[EnforcementState]<>1001,'Installation Success',
IIf([EnforcementState]>=2000 And [EnforcementState]<3000,'In Progress',
IIf([EnforcementState]>=3000 And [EnforcementState]<4000,'Requirements Not Met ',
IIf([EnforcementState]>=4000 And [EnforcementState]<5000,'Unknown',
IIf([EnforcementState]>=5000 And [EnforcementState]<6000,'Error','Unknown')))))) AS Status
FROM dbo.v_R_System AS vrs
INNER JOIN (dbo.vAppDeploymentResultsPerClient
INNER JOIN v_CIAssignment
ON dbo.vAppDeploymentResultsPerClient.AssignmentID =v_CIAssignment.AssignmentID)
ON vrs.ResourceID =dbo.vAppDeploymentResultsPerClient.ResourceID
INNER JOIN dbo.fn_ListApplicationCIs(1033) AS lac
ON lac.ci_id=dbo.vAppDeploymentResultsPerClient.CI_ID
INNER JOIN dbo.v_GS_WORKSTATION_STATUS AS vgws
ON vgws.ResourceID=vrs.resourceid
INNER JOIN v_FullCollectionMembership AS  coll
ON coll.ResourceID =vrs.ResourceID
INNER JOIN dbo.v_GS_OPERATING_SYSTEM AS vgos
ON vgos.ResourceID =vrs.ResourceID
WHERE lac.DisplayName='PRD_WalkMe_Extension_x64'
and CollectionName ='WalkMe-Install-RED-Machines'
order by Status,[Computer Name]

**<ins> SQL Query to get the Deployment Summary for an Application/Package like the given name :</ins>**
select
    vapp.SoftwareName as 'Application',
    vapp.CollectionID as 'Deployment Collection',
    vc.Name as CollectionName,
    vapp.DeploymentTime,
    vapp.NumberInProgress,
    vapp.NumberOther,
    vapp.NumberUnknown,
	vapp.NumberSuccess,
    vapp.NumberErrors,
	CASE
    When vapp.NumberSuccess > 0 then CAST(Round(((vapp.NumberSuccess/CAST(vapp.NumberInProgress+vapp.NumberOther+vapp.NumberUnknown+vapp.NumberSuccess+vapp.NumberErrors as decimal(18,2))) *100) , 2,2) as Decimal(18,2))
    Else 0
    End as 'SuccessRate'
    
from [v_DeploymentSummary] vapp
join v_collection vc
    on vc.CollectionID = vapp.CollectionID
Where vapp.SoftwareName like '%PRD_BLUE_Exploris58.16.90_JV%'
order by SuccessRate


**<ins> SQL Query to get SCCM Client Version from All Systems collection :</ins>**

SELECT distinct
 SYS.Name0 as 'Machine Name',
 SYS.User_Name0 as 'Login ID',
 SYS.User_Domain0 as 'Domain',
 USR.Full_User_Name0 as 'Full Name',
 SYS.Operating_System_Name_and0 as 'OS',
 case
 when SYS.Client0 = 1 then 'Yes'
 when SYS.Client0 = 0 then 'No'
 else convert(varchar(2), SYS.Client0)
 end as 'Client Installed',
 SYS.Client_Version0 as 'Client Version'
 FROM v_R_System SYS
 join v_FullCollectionMembership FCM on SYS.ResourceID = FCM.ResourceID
 join v_Collection CN on FCM.CollectionID = CN.CollectionID
 join v_R_User USR on SYS.User_Name0 = USR.User_Name0
 WHERE CN.Name = 'All Systems' and sys.User_Domain0 = 'Retail'

**<ins> SQL Query to get name, client, domain & OS for a list of machines :</ins>**

Select Netbios_Name0,Client0,Client_Version0,Resource_Domain_OR_Workgr0,Operating_System_Name_and0 from v_R_System 
where Netbios_Name0 in ('S07982REG1',    'S07982REG10',    'S07982REG11',    'S07982REG2',    'S07982REG3',    'S07982REG4',    'S07982REG5',    'S07982REG6',    'S07982REG7',    'S07982REG8',    'S07982REG9',    'S07985REG1',    'S07985REG2',    'S07985REG3',    'S07985REG4',    'S07985REG5',    'S07985REG6',    'S07985REG7',    'S07988REG1',    'S07988REG2',    'S07988REG3',    'S07988REG4',    'S07988REG5',    'S07988REG6',    'S07988REG7',    'S07988REG8',    'S08008REG1',    'S08008REG2',    'S08008REG3',    'S08008REG4',    'S08008REG5',    'S08008REG6',    'S08008REG7',    'S08008REG8',    'S08008REG9',    'S08432REG1',    'S08432REG2',    'S08432REG3',    'S08432REG4',    'S08432REG5',    'S08432REG6',    'S08432REG7',    'S08432REG9')

**<ins> SQL Query to find Windows 10 Version :</ins>**

select v_R_System.Name0 as 'Hostname',
v_R_System.User_Name0 as 'System Username',
v_R_System.Operating_System_Name_and0 as 'Operating System',
v_GS_OPERATING_SYSTEM.BuildNumber0 as 'Windows 10 Build Number',
Case
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19045' then 'Windows 10 22H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19044' then 'Windows 10 21H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19043' then 'Windows 10 21H1'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19042' then 'Windows 10 20H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19041' then 'Windows 10 2004'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '18363' then 'Windows 10 1909'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '18362' then 'Windows 10 1903'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '17763' then 'Windows 10 1809'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '17134' then 'Windows 10 1803'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '16299' then 'Windows 10 1709'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '15063' then 'Windows 10 1703'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '14393' then 'Windows 10 1607'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '10586' then 'Windows 10 1511'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '10240' then 'Windows 10 1507'
End as 'Windows 10 Version'
from v_r_system
inner join v_gs_operating_system
on v_R_System.ResourceID=v_GS_OPERATING_SYSTEM.ResourceID
where v_R_System.Operating_System_Name_and0 like '%Microsoft Windows NT Workstation 10.0%'
and v_R_System.Full_Domain_Name0 like '%RETAIL.ADVANCESTORES.COM%'
order by v_R_System.Name0

**<ins> SQL Query to find Windows Version in a collection :</ins>**

Declare @CollectionID varchar(8)
set @CollectionID = 'AA100949'
select v_R_System.Name0 as 'Hostname',
v_R_System.User_Name0 as 'System Username',
v_R_System.Operating_System_Name_and0 as 'Operating System',
v_R_System.Full_Domain_Name0,
v_GS_OPERATING_SYSTEM.BuildNumber0 as 'Windows 10 Build Number',
Case
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19045' then 'Windows 10 22H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19044' then 'Windows 10 21H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19043' then 'Windows 10 21H1'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19042' then 'Windows 10 20H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19041' then 'Windows 10 2004'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '18363' then 'Windows 10 1909'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '18362' then 'Windows 10 1903'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '17763' then 'Windows 10 1809'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '17134' then 'Windows 10 1803'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '16299' then 'Windows 10 1709'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '15063' then 'Windows 10 1703'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '14393' then 'Windows 10 1607'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '10586' then 'Windows 10 1511'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '10240' then 'Windows 10 1507'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '7601' then 'Windows 7 SP1'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '7600' then 'Windows 7'
End as 'Windows Version'
from v_r_system
inner join v_gs_operating_system on v_R_System.ResourceID=v_GS_OPERATING_SYSTEM.ResourceID
Inner Join v_FullCollectionMembership as Vfc on v_R_System.ResourceID=vfc.ResourceID
where vfc.CollectionID = @CollectionID
order by v_R_System.Name0

**<ins> SQL Query to find Windows Server Version :</ins>**

select v_R_System.Name0 as 'Hostname',
v_R_System.User_Name0 as 'System Username',
v_R_System.Operating_System_Name_and0 as 'Operating System',
v_GS_OPERATING_SYSTEM.BuildNumber0 as 'Build Number',
Case
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '20348' then 'Windows Server 2022'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19042' then 'Windows Server 2019, Version 20H2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '19041' then 'Windows Server 2019, Version 2004'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '18363' then 'Windows Server 2019, Version 1909'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '17763' then 'Windows Server 2019, Version 1809'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '14393' then 'Windows Server 2016, Version 1607'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '9600' then 'Windows Server 2012 R2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '9200' then 'Windows Server 2012'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '7601' then 'Windows Server 2008 R2, Service Pack 1'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '7600' then 'Windows Server 2008 R2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '6003' then 'Windows Server 2008, Service Pack 2, Rollup KB4489887'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '6002' then 'Windows Server 2008, Service Pack 2'
when v_GS_OPERATING_SYSTEM.BuildNumber0 = '6001' then 'Windows Server 2008'
End as 'OS Version'
from v_R_System
inner join v_gs_operating_system
on v_R_System.ResourceID=v_GS_OPERATING_SYSTEM.ResourceID
where v_R_System.Operating_System_Name_and0 like '%Server%'
and v_R_System.Full_Domain_Name0 like '%RETAIL.ADVANCESTORES.COM%'
order by v_R_System.Operating_System_Name_and0, v_GS_OPERATING_SYSTEM.BuildNumber0

**<ins> SQL Query to find Primary device from the email address :</ins>**

select User_Name0,name0 from v_R_system where
user_name0 in ('heather.jones','c.moralesalvarez','audrey.haisley','jonathon.young','chris.mitchell','jennifer.vigil','grayson.mcaden','pam.chambers','jim.york','lauren.marchionni','soumya.achanta','ryan.cundiff','sarah.duff','lauren.spinelli','mike.suh','asim.inam','cosette.suica','kristen.soler','jessica.mitory','mbyrd')

**<ins> SQL Query to find Free Disk Space :</ins>**

SELECT distinctSYS.Name,LDISK.Description0,LDISK.DeviceID0,LDISK.VolumeName0,LDISK.FileSystem0,LDISK.DriveType0,
vrs.Operating_System_Name_and0,LDISK.Size0/1024 [Size(GB)],LDISK.FreeSpace0/1024 [FreeSpace(GB)],
((LDISK.FreeSpace0 *100)/(LDISK.Size0))[Free Space%]
FROM v_FullCollectionMembership_Valid asSYSjoinv_GS_LOGICAL_DISK LDISK 
on sys.ResourceID =LDISK.ResourceID
join v_R_System asvrs
on vrs.ResourceID =LDISK.ResourceID
WHERE LDISK.DriveType0 =3 
AND vrs.Resource_Domain_OR_Workgr0 like'RETAIL'
AND sys.Name like'S0%REG%'
order by[FreeSpace(GB)]

**<ins> SQL Query to get all PCs Information with IP address, subnet details, RAM </ins>**

Declare @CollectionID varchar(8)
set @CollectionID ='SMS00001'
Select
vrs.Name0,
vrs.User_Name0 as 'LastUserName',
vos.Caption0,
vos.CSDVersion0,
vos.InstallDate0,
vpb.Manufacturer0,
vgc.Model0,
vpb.SerialNumber0,
vos.LastBootUpTime0,
(vd.Size0/1024) as 'HDDSize (GB)',
(vpm.TotalPhysicalMemory0/1024/1024) as 'RAMSize (GB)',
gna.IPAddress0,
gna.DefaultIPGateway0,
gna.DHCPServer0,
case gna.DHCPEnabled0
when 1 then 'Yes'
else 'No'
end as 'DHCPEnabled',
gna.IPSubnet0,
gna.MACAddress0,
vrs.AD_Site_Name0,
Case Vrs.client0
When 1 then 'Yes'
else 'No'
End as 'Client',
Case vrs.Active0
When 1 then 'Active'
else 'No'
End as 'Active'
from v_R_System as Vrs
Inner Join v_FullCollectionMembership as Vfc on vrs.ResourceID=vfc.ResourceID
left join v_GS_NETWORK_ADAPTER_CONFIGUR as GNA on vrs.ResourceID=gna.ResourceID
left join v_GS_OPERATING_SYSTEM as VOS on vrs.ResourceID=vos.ResourceID
left join v_GS_DISK as VD on vrs.ResourceID=vd.ResourceID
left join v_GS_X86_PC_MEMORY as VPM on vrs.ResourceID=vpm.ResourceID
left join v_GS_PC_BIOS as VPB on vrs.ResourceID=vpb.ResourceID
left join v_GS_COMPUTER_SYSTEM as VGC on vrs.ResourceID=vgc.ResourceID
where vfc.CollectionID =@CollectionID
and GNA.IPAddress0 is not null
and vd.MediaType0 like 'Fixed hard disk media'
order by [RAMSize (GB)]

**<ins> SQL Query to get Hard Disk information of a collection</ins>**

Declare @CollectionID varchar(8)
set @CollectionID = 'AA100374'
Select
vrs.Name0 as 'Name',
vrs.Operating_System_Name_and0 as 'Operating System',
vld.DeviceID0 as 'Drive',
(vpm.TotalPhysicalMemory0/1024/1024) as 'RAMSize GB',
(VLD.Size0/1024) as 'HDDSize GB',
(VLD.FreeSpace0/1024) as 'FreeSpace GB',
((VLD.FreeSpace0/1024 *1.0 )/(VLD.Size0/1024 *1.0)*100) as 'FreePercent'
from v_R_System as Vrs
Inner Join v_FullCollectionMembership as Vfc on vrs.ResourceID=vfc.ResourceID
left join v_GS_LOGICAL_DISK as VLD on vrs.ResourceID=VLD.ResourceID
left join v_GS_X86_PC_MEMORY as VPM on vrs.ResourceID=vpm.ResourceID
left join v_GS_OPERATING_SYSTEM as VOS on vrs.ResourceID=vos.ResourceID
where vfc.CollectionID = @CollectionID
and VLD.Description0 like 'Local Fixed Disk'
order by FreePercent

**<ins>Working Query to get Free Disk Space:</ins>**
Select DISTINCT
  SYS.Name0 as 'Computer Name',
  SYS.Operating_System_Name_and0 as 'Operating System',
  DSK.DeviceID0 as 'Drive Letter',
  DSK.Size0/1024 as 'Total Space (GB)',
  DSK.FreeSpace0/1024 as 'Free Space (GB)',
  ((DSK.FreeSpace0/1024 *1.0 )/(DSK.Size0/1024 *1.0)*100) as 'FreePercent'
 FROM
 v_R_System as SYS
  INNER JOIN v_GS_LOGICAL_DISK DSK on DSK.ResourceID =sys.ResourceID
  INNER JOIN v_FullCollectionMembership col on col.ResourceID =sys.ResourceID
  WHERE
  DSK.DriveType0=3 
  AND col.CollectionID ='SMSDM003'
  AND DSK.deviceid0='N:'
  AND DSK.Size0 >0
  AND SYS.Full_Domain_Name0 like '%Retail%'
  AND SYS.Name0 like's%'
order by[Computer Name]

**<ins> SQL Query to find Last Heartbeat Time Stamp of SCCM Clients:</ins>**

Select
vrs.Netbios_Name0 as 'ComputerName',
vrs.Client0 as 'Client',
vrs.Full_Domain_Name0 as 'Domain',
vrs.Operating_System_Name_and0 as 'Operating System',
Vad.AgentTime as 'LastHeartBeatTime'
from v_R_System as Vrs innerjoin v_AgentDiscoveries as Vad on Vrs.ResourceID=Vad.ResourceId
where vad.AgentName like '%Heartbeat Discovery'
and vrs.Full_Domain_Name0 like '%retail%'
Order by vad.AgentTime

**<ins> SQL Query to find IP Addresses of BLUE machines:</ins>**

Select vrs.Netbios_Name0,ip.IP_Addresses0,
vrs.Operating_System_Name_and0,vrs.Full_Domain_Name0,vrs.Resource_Domain_OR_Workgr0
from v_RA_System_IPAddresses ASIP
inner joinv_R_System AS vrs
on ip.ResourceID = vrs.ResourceID
where vrs.Resource_Domain_OR_Workgr0 ISNOTNULL
AND vrs.Full_Domain_Name0 ISNULL
AND ip.IP_Addresses0 like'10%'
order byvrs.Resource_Domain_OR_Workgr0,vrs.Netbios_Name0

**<ins> SQL Query to find Hardware Inv details and Client Version SQL Query:</ins>**

SELECT
S.Name0 as Name,
S.Client_Version0 as [Client Version],
OS.Caption0 as [Operating System],
CS.Manufacturer0 as Manufacturer,
CS.Model0 as Model,
PB.SMBIOSBIOSVersion0 as BIOS,
PB.SerialNumber0,
PR.Name0 as CPU,
SUM(PM.Capacity0/1024) as [Memory (GB)],
SUM(LD.Size0) as [Disc Size],
SUM(LD.FreeSpace0) as [Disc Free]
FROM
v_R_System S
join v_GS_OPERATING_SYSTEM OS on S.ResourceID = OS.ResourceID
join v_GS_PHYSICAL_MEMORY PM on S.ResourceID = PM.ResourceID
join v_GS_COMPUTER_SYSTEM CS on S.ResourceID = CS.ResourceID
join v_GS_LOGICAL_DISK LD on S.ResourceID = LD.ResourceID
join v_GS_PC_BIOS PB on S.ResourceID = PB.ResourceID
join v_GS_PROCESSOR PR on S.ResourceID = PR.ResourceID
WHERE LD.DeviceID0 ='C:'
AND S.Name0 like's0%'
GROUP BY S.ResourceID,S.Name0,S.Client_Version0,OS.Caption0,CS.Manufacturer0,CS.Model0,PB.SMBIOSBIOSVersion0,PR.Name0,PB.SerialNumber0
*****************************************************************
Declare @CollectionID varchar(8)
set @CollectionID = 'AA100042'
SELECT
S.Name0 as Name,
S.Client_Version0 as [Client Version],
OS.Caption0 as [Operating System],
S.Full_Domain_Name0,
CS.Manufacturer0 as Manufacturer,
CS.Model0 as [Product ID],
PB.SMBIOSBIOSVersion0 as BIOS,
PB.SerialNumber0,
PR.Name0 as CPU,
SUM(PM.Capacity0/1024) as [Memory (GB)],
LD.DeviceID0 as 'Drive Letter',
LD.Size0/1024 as 'Total Space (GB)',
LD.FreeSpace0/1024 as 'Free Space (GB)'
FROM
v_R_System S
join v_GS_OPERATING_SYSTEM OS on S.ResourceID = OS.ResourceID
join v_GS_PHYSICAL_MEMORY PM on S.ResourceID = PM.ResourceID
join v_GS_COMPUTER_SYSTEM CS on S.ResourceID = CS.ResourceID
join v_GS_LOGICAL_DISK LD on S.ResourceID = LD.ResourceID
join v_GS_PC_BIOS PB on S.ResourceID = PB.ResourceID
join v_GS_PROCESSOR PR on S.ResourceID = PR.ResourceID
Inner Join v_FullCollectionMembership as Vfc on S.ResourceID=vfc.ResourceID
WHERE LD.DeviceID0 ='C:'
AND vfc.CollectionID = @CollectionID
GROUP BY S.ResourceID,S.Name0,S.Client_Version0,OS.Caption0,CS.Manufacturer0,CS.Model0,PB.SMBIOSBIOSVersion0,PR.Name0,S.Full_Domain_Name0,LD.DeviceID0,LD.Size0,LD.FreeSpace0,PB.SerialNumber0
*****************************************************************
Updated Query with more details
*****************************************************************
Declare @CollectionID varchar(8)
set @CollectionID = 'AA100756'
SELECT
CN.Name as 'Collection Name',
S.Name0 as Name,
S.User_Name0 as 'System Username',
S.Operating_System_Name_and0 as 'Operating System Family',
OS.Caption0 as [Operating System],
OS.BuildNumber0 as 'Build Number',
Case
when OS.BuildNumber0 = '19045' then 'Windows 10 22H2'
when OS.BuildNumber0 = '19044' then 'Windows 10 21H2'
when OS.BuildNumber0 = '19043' then 'Windows 10 21H1'
when OS.BuildNumber0 = '19042' then 'Windows 10 20H2'
when OS.BuildNumber0 = '19041' then 'Windows 10 2004'
when OS.BuildNumber0 = '18363' then 'Windows 10 1909'
when OS.BuildNumber0 = '18362' then 'Windows 10 1903'
when OS.BuildNumber0 = '17763' then 'Windows 10 1809'
when OS.BuildNumber0 = '17134' then 'Windows 10 1803'
when OS.BuildNumber0 = '16299' then 'Windows 10 1709'
when OS.BuildNumber0 = '15063' then 'Windows 10 1703'
when OS.BuildNumber0 = '14393' then 'Windows 10 1607'
when OS.BuildNumber0 = '10586' then 'Windows 10 1511'
when OS.BuildNumber0 = '10240' then 'Windows 10 1507'
when OS.BuildNumber0 = '9600' then 'Server 2012 R2 Standard'
when OS.BuildNumber0 = '7601' then 'Windows 7 SP1'
when OS.BuildNumber0 = '7600' then 'Windows 7'
End as 'Windows Version',
case
 when S.Client0 = 1 then 'Yes'
 when S.Client0 = 0 then 'No'
 else convert(varchar(2), S.Client0)
 end as 'Client Installed',
S.Client_Version0 as [Client Version],
S.Full_Domain_Name0,
CS.Manufacturer0 as Manufacturer,
CS.Model0 as [Product ID],
PB.SMBIOSBIOSVersion0 as BIOS,
PB.SerialNumber0,
PR.Name0 as CPU,
SUM(PM.Capacity0/1024) as [Memory (GB)],
LD.DeviceID0 as 'Drive Letter',
LD.Size0/1024 as 'Total Space (GB)',
LD.FreeSpace0/1024 as 'Free Space (GB)'
FROM
v_R_System S
join v_GS_OPERATING_SYSTEM OS on S.ResourceID = OS.ResourceID
join v_GS_PHYSICAL_MEMORY PM on S.ResourceID = PM.ResourceID
join v_GS_COMPUTER_SYSTEM CS on S.ResourceID = CS.ResourceID
join v_GS_LOGICAL_DISK LD on S.ResourceID = LD.ResourceID
join v_GS_PC_BIOS PB on S.ResourceID = PB.ResourceID
join v_GS_PROCESSOR PR on S.ResourceID = PR.ResourceID
Inner Join v_FullCollectionMembership as Vfc on S.ResourceID=vfc.ResourceID
join v_Collection CN on Vfc.CollectionID = CN.CollectionID
WHERE LD.DeviceID0 ='C:'
AND vfc.CollectionID = @CollectionID
GROUP BY S.ResourceID,S.Name0,S.Client_Version0,OS.Caption0,CS.Manufacturer0,CS.Model0,PB.SMBIOSBIOSVersion0,PR.Name0,S.Full_Domain_Name0,LD.DeviceID0,LD.Size0,LD.FreeSpace0,PB.SerialNumber0,S.User_Name0,S.Operating_System_Name_and0,OS.BuildNumber0,S.Client0,CN.Name


**<ins> SQL Query to find ARP Entry against a collection ID:</ins>**

Declare @CollectionID varchar(8)
set @CollectionID = 'AA100803'
SELECT DISTINCT  
  SYS.Name0,
  ARP.DisplayName0 As 'Software Name',
  ARP.Version0 As 'Version',
  ARP.InstallDate0 As 'Installed Date'
 FROM 
  dbo.v_R_System As SYS
  INNER JOIN v_FullCollectionMembership FCM On FCM.ResourceID = SYS.ResourceID 
  INNER JOIN v_Add_REMOVE_PROGRAMS As ARP On SYS.ResourceID = ARP.ResourceID 
 WHERE   
 ARP.DisplayName0 LIKE '%Nessus%'
 AND FCM.CollectionID = @CollectionID
 ORDER BY Name0 ASC
 
**<ins> SQL Query to find OS, RAM, CPU, Cores & Hard Disk:</ins>**

SELECT
S.Name0 as Name,
OS.Caption0 as [Operating System],
CS.Manufacturer0 as Manufacturer,
CS.Model0 as Model,
PR.Name0 as CPU,
PR.NumberOfCores0 as [Number Of Cores],
PR.NumberOfLogicalProcessors0 as [Number Of Logical Processors],
SUM(PM.Capacity0/1024) as [Memory (GB)],
LD.DeviceID0 as Drive,
LD.Size0/1024 as [Disc Size (GB)],
LD.FreeSpace0/1024 as [Free Space (GB)]
FROM
v_R_System S
join v_GS_OPERATING_SYSTEM OS on S.ResourceID = OS.ResourceID
join v_GS_PHYSICAL_MEMORY PM on S.ResourceID = PM.ResourceID
join v_GS_COMPUTER_SYSTEM CS on S.ResourceID = CS.ResourceID
join v_GS_LOGICAL_DISK LD on S.ResourceID = LD.ResourceID
join v_GS_PC_BIOS PB on S.ResourceID = PB.ResourceID
join v_GS_PROCESSOR PR on S.ResourceID = PR.ResourceID
WHERE LD.DeviceID0 ='C:'
AND S.Name0 like's00%'
GROUP BY S.Name0,OS.Caption0,CS.Manufacturer0,CS.Model0,PR.Name0,PR.NumberOfCores0,PR.NumberOfLogicalProcessors0,LD.Size0,LD.FreeSpace0,LD.DeviceID0
ORDER BY S.Name0

**<ins> SQL Query to find Laptops, Desktops based on Chassis Type:</ins>**

Declare @CollectionID varchar(8)
set @CollectionID = 'AA100026'
SELECT
S.Name0 as Name,
S.User_Name0 as 'System Username',
VGS.SystemRole0 as 'System Role',
ENC.ChassisTypes0 as 'Chassis Type',
OS.Caption0 as [Operating System],
Case
when OS.BuildNumber0 = '19045' then 'Windows 10 22H2'
when OS.BuildNumber0 = '19044' then 'Windows 10 21H2'
when OS.BuildNumber0 = '19043' then 'Windows 10 21H1'
when OS.BuildNumber0 = '19042' then 'Windows 10 20H2'
when OS.BuildNumber0 = '19041' then 'Windows 10 2004'
when OS.BuildNumber0 = '18363' then 'Windows 10 1909'
when OS.BuildNumber0 = '18362' then 'Windows 10 1903'
when OS.BuildNumber0 = '17763' then 'Windows 10 1809'
when OS.BuildNumber0 = '17134' then 'Windows 10 1803'
when OS.BuildNumber0 = '16299' then 'Windows 10 1709'
when OS.BuildNumber0 = '15063' then 'Windows 10 1703'
when OS.BuildNumber0 = '14393' then 'Windows 10 1607'
when OS.BuildNumber0 = '10586' then 'Windows 10 1511'
when OS.BuildNumber0 = '10240' then 'Windows 10 1507'
when OS.BuildNumber0 = '9600' then 'Server 2012 R2 Standard'
when OS.BuildNumber0 = '7601' then 'Windows 7 SP1'
when OS.BuildNumber0 = '7600' then 'Windows 7'
End as 'Windows Version',
case
 when S.Client0 = 1 then 'Yes'
 when S.Client0 = 0 then 'No'
 else convert(varchar(2), S.Client0)
 end as 'Client Installed',
S.Client_Version0 as [Client Version],
S.Full_Domain_Name0,
CS.Manufacturer0 as Manufacturer,
CS.Model0 as [Product ID],
PB.SerialNumber0
FROM
v_R_System S
join v_GS_SYSTEM VGS on S.ResourceID = VGS.ResourceID
join v_GS_OPERATING_SYSTEM OS on S.ResourceID = OS.ResourceID
join v_GS_PHYSICAL_MEMORY PM on S.ResourceID = PM.ResourceID
join v_GS_COMPUTER_SYSTEM CS on S.ResourceID = CS.ResourceID
join v_GS_LOGICAL_DISK LD on S.ResourceID = LD.ResourceID
join v_GS_PC_BIOS PB on S.ResourceID = PB.ResourceID
join v_GS_PROCESSOR PR on S.ResourceID = PR.ResourceID
Inner Join v_FullCollectionMembership as Vfc on S.ResourceID=vfc.ResourceID
join v_Collection CN on Vfc.CollectionID = CN.CollectionID
join v_GS_SYSTEM_ENCLOSURE ENC on ENC.ResourceID = VGS.ResourceID
GROUP BY S.ResourceID,S.Name0,S.Client_Version0,OS.Caption0,CS.Manufacturer0,CS.Model0,PB.SMBIOSBIOSVersion0,PR.Name0,S.Full_Domain_Name0,LD.DeviceID0,LD.Size0,LD.FreeSpace0,PB.SerialNumber0,S.User_Name0,S.Operating_System_Name_and0,OS.BuildNumber0,S.Client0,VGS.SystemRole0,ENC.ChassisTypes0
ORDER BY ChassisTypes0
Chassis Type	Chassis Type value
1	Other
2	Unknown
3	Desktop
4	Low Profile Desktop
5	Pizza Box
6	Mini Tower
7	Tower
8	Portable
9	Laptop
10	Notebook
11	Hand Held
12	Docking Station
13	All in One
14	Sub Notebook
15	Space-Saving
16	Lunch Box
17	Main System Chassis
18	Expansion Chassis
19	SubChassis
20	Bus Expansion Chassis
21	Peripheral Chassis
22	Storage Chassis
23	Rack Mount Chassis
24	Sealed-Case PC


**<ins> List clients with hardware inventory scans more than five days old against a collection ID:</ins>**

DECLARE @CollectionID varchar(8)
SET @CollectionID = 'AA100026'
SELECT SYS.Netbios_Name0 as 'Computer Name', 
    SIS.SMS_Installed_Sites0 as 'SMS Site', WS.LastHWScan, 
    DATEDIFF(day,WS.LastHWScan,GETDATE()) as 'Days Since HWScan' 
    FROM v_GS_WORKSTATION_STATUS WS INNER JOIN v_R_System SYS 
    ON WS.ResourceID = SYS.ResourceID INNER JOIN v_RA_System_SMSInstalledSites SIS 
    ON WS.ResourceID = SIS.ResourceID INNER JOIN v_FullCollectionMembership as VFC 
    ON SYS.ResourceID = VFC.ResourceID
    WHERE SYS.Client_Type0 = 1 AND SYS.Active0 = 1 AND 
    VFC.CollectionID = @CollectionID AND
    WS.LastHWScan < DATEADD([day],-5,GETDATE())
    ORDER BY WS.LastHWScan
SQL Query to find SCCM Client ONLINE Status against a collection ID:
Select distinct fcm.CollectionID,col.Name as 'Collection Name',fcm.SiteCode,vrs.Netbios_Name0 as Name,fcm.Domain,
Case Vrs.client0 When 1 then 'Yes' else 'No' End as 'Client',
--Case vrs.Active0 When 1 then 'Active' else 'No' End as 'Active',
Case vcc.CNIsOnline When 1 then 'ONLINE' else 'OFFLINE' End as 'DeviceStatus'
from v_R_System vrs
INNER JOIN v_FullCollectionMembership as fcm on fcm.ResourceID = vrs.ResourceID
INNER JOIN v_CollectionMemberClientBaselineStatus as vcc on vcc.MachineID = vrs.ResourceID
INNER JOIN v_Collection as col on fcm.CollectionID = col.CollectionID 
where col.CollectionID like'AA10094C'
ORDER BY vrs.Netbios_Name0


**<ins> SQL Query to get the PostGre tables in descending:</ins>**

SELECT *, pg_size_pretty(total_bytes) AS total
, pg_size_pretty(index_bytes) AS INDEX
, pg_size_pretty(toast_bytes) AS toast
, pg_size_pretty(table_bytes) AS TABLE
FROM (
SELECT *, total_bytes-index_bytes-COALESCE(toast_bytes,0) AS table_bytes 
FROM (
  SELECT c.oid,nspname AS table_schema, relname AS TABLE_NAME
          , c.reltuples AS row_estimate
          , pg_total_relation_size(c.oid) AS total_bytes
          , pg_indexes_size(c.oid) AS index_bytes
          , pg_total_relation_size(reltoastrelid) AS toast_bytes
      FROM pg_class c
      LEFT JOIN pg_namespace n ON n.oid = c.relnamespace
      WHERE relkind = 'r'
  ) a
) a ORDER BY total_bytes DESC;

**<ins> SQL Query to get SCCM application and deployment information :</ins>**
Select distinct
 
    Apps.DisplayName,
    DT.DisplayName AS [DeploymentTypeName],
    DT.Technology,
    fn.Name as [Location]
    --,PC.[Owner]
    --,PC.[Support Contact]
    --,PC.[Description]
    --,Apps.NumberOfDeploymentTypes
    --,Apps.HasContent
    --,DT.PriorityInLatestApp
    --,ISNULL(DT.SDMPackageDigest.value('(/AppMgmtDigest/DeploymentType/Installer/InstallAction/Args/Arg[@Name="Description"])[1]','nvarchar(MAX)'), 'None') AS [Decription]
    --,ISNULL(DT.SDMPackageDigest.value('(/AppMgmtDigest/DeploymentType/Installer/InstallAction/Args/Arg[@Name="InstallCommandLine"])[1]','nvarchar(MAX)'), 'None') AS [InstallCommand]
    --,ISNULL(DT.SDMPackageDigest.value('(/AppMgmtDigest/DeploymentType/Installer/UninstallAction/Args/Arg[@Name="InstallCommandLine"])[1]','nvarchar(MAX)'), 'None') AS  [UninstallCommand]
    ,aa.CollectionName
    ,aa.AssignmentName
 
    From fn_ListApplicationCIs(1033) Apps
Left Join fn_ListDeploymentTypeCIs(1033) DT ON DT.AppModelName = Apps.ModelName
Left join vFolderMembers fm on apps.ModelName     = fm.InstanceKey 
Left join vSMS_Folders   fn on fm.ContainerNodeID = fn.ContainerNodeID
LEFT JOIN (
                     
Select ISNULL(Apps.SDMPackageDigest.value('(/AppMgmtDigest/Application/DisplayInfo/Info/Description)[1]','nvarchar(MAX)'), 'None') AS [Description] 
                ,Apps.SDMPackageDigest.value('(/AppMgmtDigest/Application/Owners/User/@Id)[1]', 'nvarchar(MAX)')  [Owner]
                ,Apps.SDMPackageDigest.value('(/AppMgmtDigest/Application/Contacts/User/@Id)[1]', 'nvarchar(MAX)')  [Support Contact]
                ,Apps.CI_ID from  fn_ListApplicationCIs(1033) Apps ) AS [PC] ON PC.CI_ID = Apps.CI_ID 
left join v_ApplicationAssignment aa on aa.ApplicationName = apps.DisplayName
 
 
Where Apps.IsLatest = 1 AND DT.IsLatest = 1 AND HasContent = 1 AND PriorityInLatestApp = 1 AND (fm.ObjectTypeName = N'SMS_ApplicationLatest' ) and aa.AssignmentName is not null

**<ins> SQL Query to find days since Last Communication for a collection:</ins>**

Declare @CollectionID varchar(8)
set @CollectionID = 'AAP001FD'
SELECT 
    sys.Name0 AS ComputerName,
    sys.User_Name0 AS LastLoggedOnUser,
    sys.Operating_System_Name_and0 AS OperatingSystem,
    sys.Client0 AS SCCMClient,
    CASE 
        WHEN sys.Last_Logon_Timestamp0 IS NOT NULL 
        THEN DATEDIFF(day, sys.Last_Logon_Timestamp0, GETDATE())
        ELSE NULL 
    END AS DaysSinceLastCommunication,
    sys.Last_Logon_Timestamp0 AS LastCommunicationDate,
    sys.Client_Version0 AS ClientVersion
FROM 
    v_R_System sys
    INNER JOIN v_FullCollectionMembership as fcm on fcm.ResourceID = sys.ResourceID
    INNER JOIN v_Collection CN on fcm.CollectionID = CN.CollectionID
WHERE
    sys.Client0 = 1  -- Only SCCM managed devices
    AND sys.Name0 like 'S0%'
    AND sys.Obsolete0 = 0  -- Exclude obsolete records
    AND fcm.CollectionID = @CollectionID
ORDER BY 
    DaysSinceLastCommunication DESC;


