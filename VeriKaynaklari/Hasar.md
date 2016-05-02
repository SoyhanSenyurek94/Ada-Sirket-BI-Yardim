<h1>Hasar</h1>
Hasar kayıtlarına erişmek için kullanılır. 

<a href="../Tablolar/SPOLICE.md">SHASAR</a> tablosunu kullanır.

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
<td><a href="../VeriKaynaklari/Hasar.md">Hasar</a></td>
<td>PoliceKey</td>
</tr>
<tr>
<td><a href="../VeriKaynaklari/HasarTur.md">HasarTur</a></td>
<td>HasarTurKey</td>
<td><a href="../VeriKaynaklari/Hasar.md">Hasar</a></td>
<td>HasarTurKey</td>
</tr>
<tr>
<td><a href="../VeriKaynaklari/DosyaTur.md">DosyaTur</a></td>
<td>DosyaTurKey</td>
<td><a href="../VeriKaynaklari/Hasar.md">Hasar</a></td>
<td>DosyaTurKey</td>
</tr>
</table>


<h2>Power Query</h2>
<pre>
let
    config = let
        Source = Xml.Tables(File.Contents("C:\Power BI Raporlar\config.xml")),
        Table0 = Source{0}[Table],
        Table1 = Table.TransformColumnTypes(Table0,{{"server", type text}, {"database", type text}}),
        config = Table1{0},
        veritabani = Sql.Database(config[server], config[database]),
        #"Changed Type" = Table.TransformColumnTypes(Source,{{"server", type text}, {"database", type text}})
    in
        veritabani,
        dbo_SHASAR = config{[Schema="dbo",Item="SHASAR"]}[Data],
        #"Removed Other Columns" = Table.SelectColumns(dbo_SHASAR,{"HYIL", "HACENTA", "HBRANS", "HPOLICE_NO", "HTECDIT_NO", "HKOD", "HILKODU", "HDOSYA_NO", "HHTARIH", "HTURU", "HIHBAR_TAR", "HIHBAR_SAA", "HDURUM", "HKAPANIS", "FHASNO", "FDOSYATUR", "BOLGEKODU", "FUW_YEAR", "HZEYL_NO"}),
        #"Added Custom" = Table.AddColumn(#"Removed Other Columns", "PoliceKey", each [HACENTA]&"_"&[HBRANS]&"_"&[HPOLICE_NO]&"_"&[HTECDIT_NO]&"_"&[HZEYL_NO]),
        #"Added Custom1" = Table.AddColumn(#"Added Custom", "HasarKey", each [HKOD]&"_"&[HILKODU]&"_"&[HDOSYA_NO]&"_"&[FHASNO]),
        #"Added Custom2" = Table.AddColumn(#"Added Custom1", "HasarTurKey", each "HasarTuru_"&[HKOD]&"_"&[HTURU]),
        #"Added Custom3" = Table.AddColumn(#"Added Custom2", "DosyaTurKey", each "DosyaTuru_"&[HKOD]&"_"+[FDOSYATUR])
in
    #"Added Custom3"
</pre>

<h2>Formüller</h2>

<h4>HasarTur</h4>
Hasar türünü belirtir. İhtiyaç duyulan ilişkili tablo <a href="../VeriKaynaklari/HasarTur.md">HasarTur</a>
<pre>HasarTur = RELATED(HasarTur[Hasar Nedeni])</pre>


<h4>DosyaTur</h4>
Dosya türünü belirtir. İhtiyaç duyulan ilişkili tablo <a href="../VeriKaynaklari/DosyaTur.md">DosyaTur</a>
<pre>DosyaTur = RELATED(DosyaTur[METIN])</pre>
