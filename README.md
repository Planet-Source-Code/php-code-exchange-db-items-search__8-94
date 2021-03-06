﻿<div align="center">

## DB Items Search


</div>

### Description

A quick and dirty search script for pulling catalog items from a MySQL database. Supports the use of 'And' or 'Or' or 'Not' between keywords. by Kevin Clevenger.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[PHP Code Exchange](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/php-code-exchange.md)
**Level**          |Intermediate
**User Rating**    |3.8 (15 globes from 4 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Databases](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases__8-5.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/php-code-exchange-db-items-search__8-94/archive/master.zip)





### Source Code

```
<?
/*
 Kevin Clevenger, 1999-05-23
 This is just a quick and dirty script for pulling catalog items
 from a MySQL database. This script supports the use of 'And' or 'Or'
 or 'Not' between keywords.
 Config.inc consists of
 $dbname   = "db_name";
 $dbserver  = "localhost";
 $dbuser   = "uid";
 $dbpass   = "password";
 If you make improvements to this script please mail a copy to
 ksc@wanetwork.net
*/
<table border=0 cellpadding=4 align=center width=90%>
<tr><td colspan=2 align=center><font size="6">Search</font></td></tr>
<?
 if ($search) {
  echo "<tr><td colspan=2 align=center>Search for: $search</td></tr><tr colspan=2><td></td></tr>";
  include("config.inc");
  $arrSearch = explode(" ", $search);
  for ($i=0; $i<count($arrSearch); $i++) {
   if (strToUpper($arrSearch[$i])=='AND' or strToUpper($arrSearch[$i])=='OR' or strToUpper($arrSearch[$i])=='NOT') {
    if (strToUpper($arrSearch[$i])=='NOT') {
     $i++;
     $strWhere = $strWhere." and descr not like '%".$arrSearch[$i]."%'";
	} else {
     $strWhere = $strWhere." ".$arrSearch[$i]." ";
	}
   } else {
    $strWhere = $strWhere."descr like '%".$arrSearch[$i]."%'";
   }
  }
  $cn=mysql_connect($dbserver, $dbuser, $dbpass);
  mysql_select_db($dbname,$cn);
  $sql="SELECT * FROM items WHERE ".$strWhere." ORDER BY name";
  $rsCat_query=mysql_query($sql, $cn);
  if (!(mysql_errno()==0)) {
   echo "<tr><td colspan=2 align=center><big>There was a problem with the query syntax</big></td></tr>";
   echo "<tr><td colspan=2 align=center><a href=search.php3>Back</a></td></tr></table>";
   exit;
  }
  if (mysql_num_rows($rsCat_query)==0) {
   echo "<tr><td colspan=2 align=center><big>No items were found matching the criteria</big></td></tr>";
   echo "<tr><td colspan=2 align=center><a href=search.php3>Back</a></td></tr></table>";
   exit;
  }
  while($rsCat = mysql_fetch_array($rsCat_query)) {
?>
  <tr><td>Iterate through your fields here</td></tr>
  <tr><td>Field_1 Value:</td><td><? echo $rsCat["field1_name"] ?></td></tr>
  <tr><td>Field_2 Value:</td><td><? echo $rsCat["field2_name"] ?></td></tr>
  <tr><td>Field_3 Value:</td><td><? echo $rsCat["field3_name"] ?></td></tr>
<?
 }
} else {
?>
<form action=search.php3 method=post>
<tr><td align=center colspan=2>&nbsp;&nbsp;<input type=text name=search id=search size=25>
 <input type=submit name=submit value=Submit></td></tr>
<tr><td colspan=2 align=center>Enter the criteria you wish to search for. The search is not case sensitive.</td></tr>
<tr><td colspan=2 align=center>You may use 'And' or 'Or' or 'Not' between keywords.</td></tr>
<tr><td align=right width=100>Example 1:</td><td>this and that</td></tr>
<tr><td align=right>Example 2:</td><td>this not that</td></tr>
<tr><td align=right>Example 3:</td><td>this and that not those</td></tr>
<tr><td align=right>Example 4:</td><td>this and that or those</td></tr>
</form>
<? } ?>
</table>
```

