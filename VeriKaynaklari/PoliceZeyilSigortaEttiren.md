<h1>PoliceZeyilSigortaEttiren</h1>
Poliçe ve zeyillerin sigorta ettiren ayrıntılarına erişmek için kullanılır. 

<a href="../Tablolar/ASW_SIGORTA_ETTIREN.md">ASW_SIGORTA_ETTIREN</a> tablosunu kullanır.

<h2>İlişki Kurulabilecek Veri Kaynakları</h2>
<table>
<tr>
<th>Ana Veri Kaynağı</th>
<th>Ana Veri Kaynağı Kolon Adı</th>
<th>Çocuk Veri Kaynağı</th>
<th>Çocuk Veri Kaynağı Kolon Adi</th>
</tr>
<tr>
<td><a href="../VeriKaynaklari/PoliceZeyil.md">PoliceZeyil</a></td>
<td>PoliceKey</td>
<td><a href="../VeriKaynaklari/PoliceZeyilSigortaEttiren.md">PoliceZeyilSigortaEttiren</a></td>
<td>PoliceKey</td>
</tr>
</table>


<h2>Power Query</h2>
<pre>
let
    Source = Xml.Tables(File.Contents("C:\Power BI Raporlar\config.xml")),
    Table0 = Source{0}[Table],
    Table1 = Table.TransformColumnTypes(Table0,{{"server", type text}, {"database", type text}}),
    config = Table1{0},
    veritabani = Sql.Database(config[server], config[database], [Query="select * from ASW_SIGORTA_ETTIREN SE where exists (select '' from SPOLICE p where p.acenta = SE.ACENTE and p.BRANS = SE.BRANS AND P.POLICE_NO = SE.POLICE_NO AND P.TECDIT_NO = SE.TECDIT_NO AND P.ZEYL_NO=SE.ZEYIL_NO AND P.IPT_KAYIT IN ('K','I'))"]),
    #"Removed Other Columns" = Table.SelectColumns(veritabani,{"ACENTE", "BRANS", "POLICE_NO", "TECDIT_NO", "ZEYIL_NO", "DOGUM_TARIHI", "CINSIYET", "IL_KODU", "ILCE_KODU", "UYRUK", "OLUM_TARIHI"}),
    #"Added Custom" = Table.AddColumn(#"Removed Other Columns", "PoliceKey", each [ACENTE]&"_"&[BRANS]&"_"&[POLICE_NO]&"_"&[TECDIT_NO]&"_"&[ZEYIL_NO])
in
    #"Added Custom"
</pre>

<h2>Formüller</h2>

