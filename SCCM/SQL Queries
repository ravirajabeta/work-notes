<ins>Collection Query Rule to add the list of machines :</ins>
select * from SMS_R_System where SMS_R_System.Name in ("5CG5270G4B",	"5CG4453ZNS",	"5CG533795N",	"5CG6255913",	"5CG4454HM3",	"5CG4483FMP",	"CNU431BKFX",	"5CG529400Y",	"5CG5337920",	"5CG52943DG",	"5CG4523D78",	"CNU25198XG",	"5CG4503XHG",	"5CG529410Z",	"5CG625580G",	"5CG4453Z90",	"5CG529424T",	"5CG7213SNK",	"5CG6255GVR",	"5CG5342Z0M",	"5CG52940JG",	"5CG5070707")

SQL Query to get the Installed Software Version on SCCM Clients:
select distinct vrs.Name0,vrs.Operating_System_Name_and0,vrs.Resource_Domain_OR_Workgr0,vga.DisplayName0,vga.Version0 
from v_R_System as vrs inner join v_GS_ADD_REMOVE_PROGRAMS as vga on vrs.ResourceID = vga.ResourceID 
where vga.DisplayName0 like 'CrowdStrike Windows Sensor' 

select distinct vrs.Name0,vrs.Operating_System_Name_and0,vrs.Resource_Domain_OR_Workgr0,vga.DisplayName0,vga.Version0 
from v_R_System as vrs inner join v_GS_ADD_REMOVE_PROGRAMS as vga on vrs.ResourceID = vga.ResourceID 
where vga.DisplayName0 like 'Citrix%'
and vrs.Resource_Domain_OR_Workgr0 like 'RETAIL'
and vrs.Name0 like 'S00962REG1'
/* and vrs.Name0 in ('S00770REG2',  'S00770REG4')  

SQL Query to list all the members of a Collection:

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
