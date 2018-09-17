<table>
  <tr>
    <th><b>Description</b></th>
    <th><b>Code</b></th>
  </tr>
  <tr>
    <td>For every single jet, take all tracks and find the closest track that has pt>1 gev, and then plot the delta-R of that track and jet, the jet's pT, and the tracks pT.</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>
      <pre lang="cpp">Draw("Muon_pt","Sum$(Muon_pt*(Muon_eta > 1)) > 30")</pre>
    </td>
  </tr>
</table>
