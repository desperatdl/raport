<!DOCTYPE HTML>
<HTML LANG="PL">
    <HEAD>
        <META CHARSET="UTF-8">
        <META http-equiv="x-UA-COMPATIBLE" content="IE=edge,chrome=1">
        <TITLE>RaportApp</TITLE>
    </HEAD>
    <BODY>
        <TABLE border="1" width="100%">
            <td align="center">Zdjecie logo</td>
        </TABLE>
        

            
       <?php
       require_once "connect.php";
?> <FORM method="post">
<TABLE border="1" width="100%">
            <TD><TABLE border="1" width="100%">
   <TR><TD>Data od : <input type="date" name='od'>
            Data do : <input type="date" name='do'>
            
            </tr>                 
   <TR><TD>
                           
Wybierz kierowce dupa: 
<select name="opcja"  style="width:250px" >
	<?php
 
	$data = mysqli_query($conn, 'SELECT * FROM raportapp_drivers');
 
	while($kategoria = mysqli_fetch_assoc($data))
	{
	echo'<option value='.$kategoria[id].'>'.$kategoria[firstname].' '.$kategoria[lastname].'</option>';
        }
         
	?>
	</select>
    <input type="submit" value="Wyślij zamówienie" /> </form>
                       </tr>
                    <TR><TD>
                    <?PHP
                   @$date_od =$_POST['od'];
                   @$date_do =$_POST['do'];
                   @$kierowca=$_POST['opcja'];
                    $result1 = mysqli_query($conn, "SELECT TRUNCATE(SUM(prowizja),2) AS sumaprowizji, TRUNCATE(SUM(obrot),2) AS sumaobrotu, TRUNCATE(SUM(przebieg),2) AS sumaprzebiegu, TRUNCATE(SUM(lpgOnLitry),2) AS sumalpgonlitry, TRUNCATE(SUM(pb95Litry),2) AS sumapb95litry, TRUNCATE(SUM(lpgOnZl),2) AS sumalpgonzl, SUM(pb95Zl) AS sumapbzl, TRUNCATE(AVG(obrotzkm),2) AS sredniaobrotzkm, TRUNCATE(SUM(kasaDoWorka),2) AS gotowka, SUM(karta) AS terminal, SUM(myTaxi) as mytaxi, SUM(iTaxi) as itaxi, SUM(dzieci) as dzieci, SUM(sprzatanieGotowka) as sprzatanie, TRUNCATE(SUM(paliwoGotowka),2) as paliwogotowka, rd.firstname, rd.lastname FROM raportapp r INNER JOIN raportapp_drivers rd ON r.id_driver = rd.id WHERE rd.id='$kierowca' AND r.date>='$date_od' AND r.date<='$date_do'");
                   
        $json2 = array();
        echo '<table border=1>';

    while (($row1 = mysqli_fetch_assoc($result1)) != NULL) 
    {
        echo '<TR><TD>Wybrany zakres to:<TD>'.$date_od.'-'.$date_do.'</td></tr>';
        echo '<TR><TD>Kierowca: <TD>'.$row1['firstname'].' '.$row1['lastname'].'</td></tr>';  
        echo '<TR><td>Suma prowizji: <TD>' . $row1['sumaprowizji'].'</td></td>';
        echo '<TR><td>Suma obrotu<td>' . $row1['sumaobrotu'].'</td>';
        echo '<tr><td>Suma przebiegu<td>' . $row1['sumaprzebiegu'].'</td>';
        echo '<tr><td>Suma LPG/ON litry<td>' . $row1['sumalpgonlitry'].'</td>';
        echo '<tR><td>Suma PB 95 litry<td>' . $row1['sumapb95litry'].'</td>';
        echo '<tr><td>Suma LPG/ON zł<td>' . $row1['sumalpgonzl'].'</td>';
        echo '<tr><td>Suma PB 95 zł<td>' . $row1['sumapbzl'].'</td>';
        echo '<tr><td>Obrót z km<td>' . $row1['sredniaobrotzkm'].'</td>';
        echo '<tr><td>Gotówka: <td>' . $row1['gotowka'].'</td>';
        echo '<tr><td>Terminal:<td>' . $row1['terminal'].'</td>';
        echo '<tr><td>mytaxi:<td>' . $row1['mytaxi'].'</td>';
        echo '<tr><td>itaxi:<td>' . $row1['itaxi'].'</td>';
        echo '<tr><td>dzieci:<td>' . $row1['dzieci'].'</td>';
        echo '<tr><td>sprzątanie(gotówka)<td>' . $row1['sprzatanie'].'</td>';
        echo '<tr><td>paliwo(gotówka)<td>' . $row1['paliwogotowka'].'</td>';
           
    echo '</tr>';
}
//echo json_encode (array('wynik2'=> $json2));


                    ?>
                    
                    </td></tr>
                </table>
                </TABLE></TD>
                <td><h2>Samochody</h2>
                <TABLE border="1" width="50%">
                <TR><TH>Opel Astra</th>
                <TD>Aktualny przebieg:<TD>
                 <?PHP
            $autko= $_POST['autko'];
            $r_autko = mysqli_query($conn,"SELECT MAX(r.stopKm) AS przebieg FROM raportapp r WHERE r.id_car = '$autko'");
            while (($row2 = mysqli_fetch_assoc($r_autko)) != NULL) 
                {
                    echo $row2['przebieg'];
                    $licznik=$row2['przebieg'];
                    $kmodwymiany=15000;
                }
            ?>
                        <TD>Do wymiany oleju zostało:<TD><?PHP echo $licznik?>
                    </TABLE></table>
                <table>
                <TD>
                <?PHP
                echo '<table border=1>';
                echo '<tr>';
                echo '<th>Data</th>';
                echo '<th>Imię</th>';
                echo '<th>Nazwisko</th>';
                echo '<th>Obrót</th>';
                echo '<th>Przebieg</th>';
                echo '<th>Prowizja</th>';
                echo '<th>Samochód</th>';
                echo '</tr>';
                $result3 = mysqli_query($conn, "SELECT
    r.date,
    rd.firstname,
    rd.lastname,
    r.obrot,
    r.przebieg,
    r.prowizja,
    rc.car
FROM
    raportapp r
INNER JOIN raportapp_drivers rd 
ON
    rd.id = r.id_driver,
    raportApp_cars rc
    where r.id_driver='$kierowca' AND r.id_car=rc.id AND r.date>='$date_od' AND r.date<='$date_do'");
            while (($row = mysqli_fetch_assoc($result3)) != NULL) {
                echo '<tr>';
                echo '<td>' . $row['date'] . '</td>';
                echo '<td>' . $row['firstname'] . '</td>';
                echo '<td>' . $row['lastname'] . '</td>';
                echo '<td>' . $row['obrot'] . '</td>';
                echo '<td>' . $row['przebieg'] . '</td>';
                echo '<td>' . $row['prowizja'] . '</td>';
                echo '<td>' . $row['car'] . '</td>';
                echo '</tr>';
}
?>
                
                    

        </TABLE>


    
    </BODY>
</HTML>
    
