<table>
  <tr>
    <th><b>Description</b></th>
    <th><b>Pseudo-code</b></th>
    <th><b>Existing solutions</b></th>
  </tr>
  <tr>
    <td>If the sum of the pt of all muons with eta greater than 1 is greater than 30, fill histogram with pt of all muons.</td>
    <td><pre lang="python">
for e in events:
  sum_pt = 0
  for m in muons:
    if m.eta > 1:
      sum_pt += m.pt
    if sum_pt > 30:
      hist.fill(muons.pt)
    </pre></td>
    <td>
      RDF
      <pre lang="cpp">df.Filter("Sum(Muon_pt[abs(Muon_eta) > 1]) > 30").Histo1D("Muon_pt")</pre>
      TTree::Draw
      <pre lang="cpp">Draw("Muon_pt","Sum$(Muon_pt*(abs(Muon_eta) > 1)) > 30")</pre>
    </td>
  </tr>
  <tr>
    <td>For all events, make an histogram of the pt of the second muon if the eta of the first
    is greater than 1.</td>
    <td><pre lang="python">
for e in events:
  if muons[1].eta > 1:
    hist.fill(muons[1].pt)
    </pre></td>
    <td>
      RDF
      <pre lang="cpp">df.Filter("Muon_eta[0] > 1").Define("Muon_pt_1", "Muon_pt[1]").Histo1D("Muon_pt_1")</pre>
      TTree::Draw
      <pre lang="cpp">Draw("Muon_pt[1]", "Muon_eta[0] > 1")</pre>
    </td>
  </tr>
  <tr>
    <td>For every event, for every Z boson candidate, look for the back-to-back jet and make an histogram of the ratio of their transverse momenta.</td>
    <td><pre lang="python">
for e in events:
  for z in z_candidates:
    for j in jets:
      if j.p == -z.p:
        hist.fill(j.pt/z.pt)
    </pre></td>
    <td>???</td>
  </tr>
  <tr>
    <td>For every event, for every jet, take all tracks and find the closest track that has pt>1 gev, and then plot the delta-R of that track and jet, the jet's pT, and the tracks pT.</td>
    <td><pre lang="python">
for e in events:
  for j in jets:
    closest_track = None
    d = None
    for t in tracks:
      if t.pt > 1:
        if d > (t.p - j.p)**2:
          d = (t.p - j.p)**2
          closest_track = t
    hist_jpt.fill(j.pt)
    hist_tpt.fill(closest_track.pt)
    hist_dr.fill(
      sqrt(j.eta**2 + j.phi**2) -
      sqrt(closest_track.eta**2 + closest_track.phi**2))
    </pre></td>
    <td>???</td>
  </tr>
  <tr>
    <td> Among all tracks, select muons and plot their p/pT distribution. </td>
    <td><pre lang="python">
    h = TH2D("h", ";P[GeV/c];p_{T}[GeV/c]", 100, 0, 100, 100, 0, 3)
    for track in tracks:
      if abs(track.GetPdgCode() == 13):
        pt = hypot(track.GetPx(), track.GetPy())
        p = hypot(track.GetPz(), pt)
        h.Fill(p, pT)
    </pre></td>
    <td>
      RDF
      <pre lang="cpp">
      df.Define("Tracks", clones_converter&lt;ShipMCTrack&gt;, {"MCTrack"})
          .Define("Muons",
                  [](const RVec&lt;ShipMCTrack&gt; &ts) {
                        return Filter(ts, [](const ShipMCTrack &t) {
                              return abs(t.GetPdgCode()) == 13;
                        });
                  },
                  {"Tracks"})
          .Define("Muons_pt",
                  [](const RVec&lt;ShipMCTrack&gt; &muons) {
                        return Map(muons, [](const ShipMCTrack &muon) {
                              return sqrt(muon.GetPx() * muon.GetPx() +
                                          muon.GetPy() * muon.GetPy());
                        });
                  },
                  {"Muons"})
          .Define("Muons_P",
                  [](const RVec&lt;ShipMCTrack&gt; &muons) {
                        return Map(muons, [](const ShipMCTrack &muon) {
                              return sqrt(muon.GetPx() * muon.GetPx() +
                                          muon.GetPy() * muon.GetPy() +
                                          muon.GetPz() * muon.GetPz());
                        });
                  },
                  {"Muons"})
          .Histo2D({"h", ";P[GeV/c];p_{T}[GeV/c]", 100, 0, 100, 100, 0, 3}, "Muons_P",
                   "Muons_pt");
      </pre>
      TTree::Draw
      <pre lang="cpp">
      tree->Draw("sqrt(MCTrack.GetPx()*MCTrack.GetPx()+MCTrack.GetPy()*MCTrack.GetPy())"
           ":sqrt(MCTrack.GetPx()*MCTrack.GetPx()+MCTrack.GetPy()*MCTrack.GetPy()+MCTrack.GetPz()*MCTrack.GetPz())"
           ">>h(100, 0, 100, 100, 0, 3)",
           "abs(MCTrack.GetPdgCode())==13",
           "colz")
      </pre>
    </td>
  </tr>
</table>
