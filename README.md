
<style>
    body {
        font-family: Arial, sans-serif;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 20px 0;
    }
    th {
        padding: 10px;
        text-align: center;
        border: 1px solid #ddd;
        background-color: #0F837D;
        color: white;
    }
    td {
        padding: 10px;
        text-align: center;
        border: 1px solid #ddd;
    }
    tr:hover {
        background-color: #f9f9f9;
    }
    .table-striped tr:nth-child(odd) {
        background-color: #f9f9f9;
    }
    .table-hover tr:hover {
        background-color: #f1f1f1;
    }
</style>
<table class="dataframe table table-striped table-hover">
  <thead>
    <tr style="text-align: center;">
      <th></th>
      <th>AM-bidrag</th>
      <th>Afgift på private erhvervs materialeinput</th>
      <th>Aktieskat</th>
      <th>Bundskat</th>
      <th>Ejendomsskat</th>
      <th>Energiafgift til privat forbrug</th>
      <th>Ikke-skattepligtige overførselsindkomster</th>
      <th>Løntilskud</th>
      <th>Moms</th>
      <th>Offentlig beskæftigelse</th>
      <th>Offentlig løn</th>
      <th>Offentlige investeringer</th>
      <th>Offentlige varekøb</th>
      <th>Offentligt forbrug</th>
      <th>Produktionssubsidier ekskl. løntilskud</th>
      <th>Produktsubsidier</th>
      <th>Registreringsafgift til privat forbrug</th>
      <th>Selskabsskat</th>
      <th>Skattepligtige overførselsindkomster</th>
      <th>Vægtafgift</th>
      <th>Øvrige forbrugsafgifter</th>
      <th>Øvrige overførsler til husholdninger</th>
    </tr>
    <tr>
      <th>Variable</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BNP (qBNP)</th>
      <td>0.1625</td>
      <td>-0.1441</td>
      <td>-0.0820</td>
      <td>0.2637</td>
      <td>0.1159</td>
      <td>0.4334</td>
      <td>0.2313</td>
      <td>0.0490</td>
      <td>0.1496</td>
      <td>0.7086</td>
      <td>1.0958</td>
      <td>0.4129</td>
      <td>0.3923</td>
      <td>0.6480</td>
      <td>0.0451</td>
      <td>0.0641</td>
      <td>0.3880</td>
      <td>0.0215</td>
      <td>0.1603</td>
      <td>0.2664</td>
      <td>0.3895</td>
      <td>0.2649</td>
    </tr>
    <tr>
      <th>BVT-deflator (pBVT)</th>
      <td>0.0736</td>
      <td>1.2117</td>
      <td>-0.0300</td>
      <td>0.1199</td>
      <td>0.0183</td>
      <td>0.1290</td>
      <td>0.1045</td>
      <td>0.0546</td>
      <td>0.2016</td>
      <td>0.2908</td>
      <td>0.0512</td>
      <td>0.1533</td>
      <td>0.1316</td>
      <td>0.2500</td>
      <td>-0.1513</td>
      <td>0.4082</td>
      <td>-0.0190</td>
      <td>0.0076</td>
      <td>0.0710</td>
      <td>0.1213</td>
      <td>0.1049</td>
      <td>0.1201</td>
    </tr>
    <tr>
      <th>Beskæftigelse (nL)</th>
      <td>0.0371</td>
      <td>-0.0241</td>
      <td>-0.0182</td>
      <td>0.0598</td>
      <td>0.0277</td>
      <td>0.0771</td>
      <td>0.0523</td>
      <td>0.1655</td>
      <td>0.0340</td>
      <td>0.5604</td>
      <td>0.0097</td>
      <td>0.0855</td>
      <td>0.0760</td>
      <td>0.3941</td>
      <td>0.0284</td>
      <td>0.0294</td>
      <td>0.0329</td>
      <td>-0.0103</td>
      <td>0.0363</td>
      <td>0.0603</td>
      <td>0.0682</td>
      <td>0.0599</td>
    </tr>
    <tr>
      <th>Boliginvesteringer (qI_s[iB,bol])</th>
      <td>0.8582</td>
      <td>0.5735</td>
      <td>0.5996</td>
      <td>1.3759</td>
      <td>1.4446</td>
      <td>-0.1316</td>
      <td>1.1606</td>
      <td>0.5887</td>
      <td>0.0251</td>
      <td>0.1378</td>
      <td>0.7184</td>
      <td>-0.3303</td>
      <td>-0.1146</td>
      <td>-0.0068</td>
      <td>0.3225</td>
      <td>0.1841</td>
      <td>-0.5154</td>
      <td>0.3611</td>
      <td>0.7634</td>
      <td>1.3418</td>
      <td>-0.1251</td>
      <td>1.3831</td>
    </tr>
    <tr>
      <th>Boligpriser (pBolig)</th>
      <td>0.7169</td>
      <td>0.2498</td>
      <td>0.4778</td>
      <td>1.1487</td>
      <td>1.1654</td>
      <td>-0.0759</td>
      <td>0.9705</td>
      <td>0.5338</td>
      <td>0.0323</td>
      <td>0.2284</td>
      <td>0.5938</td>
      <td>-0.0116</td>
      <td>-0.0562</td>
      <td>0.1206</td>
      <td>0.2120</td>
      <td>0.1245</td>
      <td>-0.4036</td>
      <td>0.2365</td>
      <td>0.6394</td>
      <td>1.1216</td>
      <td>-0.0649</td>
      <td>1.1547</td>
    </tr>
    <tr>
      <th>Eksport (qXy)</th>
      <td>-0.0511</td>
      <td>0.1039</td>
      <td>0.0101</td>
      <td>-0.0834</td>
      <td>-0.0333</td>
      <td>-0.0699</td>
      <td>-0.0726</td>
      <td>-0.0581</td>
      <td>-0.0065</td>
      <td>-0.2734</td>
      <td>-0.0223</td>
      <td>-0.0941</td>
      <td>-0.0858</td>
      <td>-0.2137</td>
      <td>0.0944</td>
      <td>0.1723</td>
      <td>-0.0381</td>
      <td>-0.0161</td>
      <td>-0.0487</td>
      <td>-0.0845</td>
      <td>-0.0136</td>
      <td>-0.0834</td>
    </tr>
    <tr>
      <th>Import (qM)</th>
      <td>0.3025</td>
      <td>0.4762</td>
      <td>-0.1582</td>
      <td>0.4923</td>
      <td>0.2030</td>
      <td>0.7325</td>
      <td>0.4312</td>
      <td>0.1704</td>
      <td>0.3360</td>
      <td>0.0494</td>
      <td>0.1719</td>
      <td>0.8492</td>
      <td>0.7612</td>
      <td>0.4430</td>
      <td>0.2268</td>
      <td>0.6548</td>
      <td>1.6523</td>
      <td>-0.0313</td>
      <td>0.2976</td>
      <td>0.4981</td>
      <td>0.6841</td>
      <td>0.4940</td>
    </tr>
    <tr>
      <th>Nominelle lønninger (vhW_DA)</th>
      <td>0.1765</td>
      <td>-0.0176</td>
      <td>-0.0767</td>
      <td>0.2875</td>
      <td>0.1340</td>
      <td>0.3914</td>
      <td>0.2504</td>
      <td>0.8469</td>
      <td>0.1831</td>
      <td>0.9918</td>
      <td>0.2179</td>
      <td>0.3869</td>
      <td>0.3387</td>
      <td>0.7958</td>
      <td>0.2142</td>
      <td>0.1939</td>
      <td>0.1643</td>
      <td>0.0021</td>
      <td>0.1707</td>
      <td>0.2905</td>
      <td>0.3562</td>
      <td>0.2881</td>
    </tr>
    <tr>
      <th>Primær saldo, andel af BNP<br>(vPrimSaldo / vBNP)</th>
      <td>-0.5127</td>
      <td>-0.8401</td>
      <td>-1.0479</td>
      <td>-0.8385</td>
      <td>-0.7159</td>
      <td>-1.0971</td>
      <td>-0.7232</td>
      <td>-0.6367</td>
      <td>-0.5563</td>
      <td>-0.6954</td>
      <td>-0.4540</td>
      <td>-0.7894</td>
      <td>-0.6958</td>
      <td>-0.8058</td>
      <td>-0.6632</td>
      <td>-0.6197</td>
      <td>-1.7459</td>
      <td>-0.8436</td>
      <td>-0.5287</td>
      <td>-0.8340</td>
      <td>-1.0441</td>
      <td>-0.8351</td>
    </tr>
    <tr>
      <th>Privat forbrug (qC)</th>
      <td>0.6581</td>
      <td>0.0131</td>
      <td>-0.4611</td>
      <td>1.0723</td>
      <td>0.2839</td>
      <td>1.8643</td>
      <td>0.9454</td>
      <td>0.2773</td>
      <td>0.7007</td>
      <td>0.1504</td>
      <td>0.5574</td>
      <td>-0.0546</td>
      <td>-0.0743</td>
      <td>0.0602</td>
      <td>0.0828</td>
      <td>0.2480</td>
      <td>3.1383</td>
      <td>0.0495</td>
      <td>0.6581</td>
      <td>1.0906</td>
      <td>1.6619</td>
      <td>1.0761</td>
    </tr>
    <tr>
      <th>Private investeringer<br>(qI_s[iM,spTot]+qIbErhverv)</th>
      <td>0.1684</td>
      <td>0.3866</td>
      <td>-0.0677</td>
      <td>0.2749</td>
      <td>0.2927</td>
      <td>0.5390</td>
      <td>0.2396</td>
      <td>0.1724</td>
      <td>0.2670</td>
      <td>-0.4695</td>
      <td>0.1044</td>
      <td>0.2175</td>
      <td>0.2548</td>
      <td>-0.1664</td>
      <td>0.5366</td>
      <td>0.4615</td>
      <td>0.1617</td>
      <td>-0.2928</td>
      <td>0.1621</td>
      <td>0.2789</td>
      <td>0.4409</td>
      <td>0.2749</td>
    </tr>
  </tbody>
</table>