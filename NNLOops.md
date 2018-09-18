<table>
  <tr>
    <th><b>Description</b></th>
    <th><b>Code</b></th>
  </tr>
  <tr>
    <td>If the sum of the pt of all muons with eta greater than 1 is greater than 30, fill histogram with pt of all muons.</td>
    <td>
      RDF
      <pre lang="cpp">df.Filter("Sum(Muon_pt[abs(Muon_eta) > 1]) > 30").Histo1D("Muon_pt")</pre>
      TTree::Draw
      <pre lang="cpp">Draw("Muon_pt","Sum$(Muon_pt*(Muon_eta > 1)) > 30")</pre>
    </td>
  </tr>
  <tr>
    <td>For all events, make an histogram of the pt of the second muon if the eta of the first
    is greater than 1.</td>
    <td>
      RDF
      <pre lang="cpp">df.Filter("Muon_eta[0] > 1").Define("Muon_pt_1", "Muon_pt[1]").Histo1D("Muon_pt_1")</pre>
      TTree::Draw
      <pre lang="cpp">Draw("Muon_pt[1]", "Muon_eta[0] > 1")</pre>
    </td>
  </tr>
  <tr>
    <td>For every event, for every jet, take all tracks and find the closest track that has pt>1 gev, and then plot the delta-R of that track and jet, the jet's pT, and the tracks pT.</td>
    <td>???</td>
  </tr>
</table>
