# BT-Tree QEC: Staircase Redundancy Demo

**Interactive demo of ultrametric quantum error correction on the Bruhat-Tits tree.**

🔗 **Live:** [qwav-demo-bt-qec.pages.dev](https://qwav-demo-bt-qec.pages.dev)  
📄 **Paper:** [DOI 10.5281/zenodo.21817596](https://doi.org/10.5281/zenodo.21817596)

---

## What This Demonstrates

Quantum error correction (QEC) and quantum Darwinism compete for the same resource: redundancy. Maity et al. proved a no-go theorem: under standard (Archimedean) assumptions, you can't have both. But what if the geometry of the code is **ultrametric** instead of Archimedean?

This demo shows the core structural prediction of the paper *Ultrametric Code Spaces: The Bruhat--Tits Tree as a Geometry for Quantum Error Correction*:

| Archimedean codes | BT-tree codes |
|:------------------|:--------------|
| Add +1 qubit = +1 redundancy | Add +1 tree level = +p^r redundancy |
| Smooth tradeoff curve | **Discrete staircase** |
| No coexistence possible | Coexistence windows between steps |

### Features

- **Tree Geometry**: Visualize the Bruhat-Tits tree for primes p = 2, 3, 5 at adjustable depth
- **Redundancy Curve**: See the staircase pattern — redundancy jumps by p^r at each level, vs. smooth Archimedean growth
- **Error Injection**: Click any node to inject an error — watch ultrametric confinement (error only affects that node's subtree, never propagates horizontally)
- **Live Metrics**: Node count, redundancy, leaf count update in real-time
- **Archimedean Comparison**: Toggle to overlay the smooth 2^d curve against the BT-tree staircase

---

## How to Use

### Controls
| Control | What it does |
|:--------|:-------------|
| **Prime buttons (2, 3, 5)** | Select the effective prime p of the BT tree |
| **Depth slider (1-6)** | Adjust how many tree levels to build |
| **Tree geometry tab** | Visual tree view with interactive nodes |
| **Redundancy curve tab** | Plot showing the staircase structure |
| **Compare Archimedean** | Overlay smooth 2^d curve (golden dashed line) |
| **Inject error at node** | Click mode: click any node to see ultrametric error confinement |
| **Animate tree growth** | Watch the tree build level by level |
| **Reset view** | Clear all selections and return to default state |

### Interpreting the Results
- **Redundancy staircase**: Each horizontal segment = redundancy constant across depths within that level. Each vertical jump = adding an entire tree level at once (+p^r distinct environment fragments).
- **Archimedean comparison**: Smooth curve with no plateaus — redundancy changes continuously.
- **Error confinement**: Injected errors spread only within a subtree (ultrametric property). Never propagate to siblings or cross subtrees — this is the geometric feature that enables QEC-Darwinism coexistence.

---

## How It Works (Technical)

### Computation Engine (`BTTree` class)
- Constructs the Bruhat-Tits tree: an infinite (p+1)-regular tree with vertices corresponding to p-adic balls
- Each node has p children; the tree has p^d nodes at depth d
- `getErrorSubtree(nodeId)` traverses the subtree and returns all affected nodes
- `getStaircase(maxDepth)` computes redundancy R(d) = p^d at each depth
- `getArchimedean(maxDepth)` computes the smooth comparison curve

### Visualization (`TreeRenderer` class)
- Canvas-based rendering with two views: tree geometry and redundancy curve
- Tree view: top-down layout with depth levels, edge routing, node coloring
- Curve view: staircase plot with filled area, step annotations, Archimedean overlay
- Hit-testing for mouse interaction (hover tooltips, click selection, error injection)

### Architecture
```
index.html  (single file, zero dependencies)
├── CSS     (dark theme, 260 lines)
├── BTTree  (computation engine, ~70 lines)
├── TreeRenderer (canvas rendering, ~200 lines)
└── App     (controller, event wiring, ~150 lines)
```

No frameworks. No build step. No dependencies. One file, open in any browser.

---

## Deployment

This demo is deployed on **Cloudflare Pages** with auto-deploy from GitHub:

| Layer | Detail |
|:------|:-------|
| **Source** | [github.com/QNFO/qwav-demo-bt-qec](https://github.com/QNFO/qwav-demo-bt-qec) |
| **Hosting** | Cloudflare Pages (`qwav-demo-bt-qec.pages.dev`) |
| **Auto-deploy** | Enabled — push to `main` branch triggers automatic deployment |
| **Preview deploys** | All branches get preview URLs |
| **Build** | No build step (static HTML) |

### To Deploy an Update
```bash
git clone https://github.com/QNFO/qwav-demo-bt-qec.git
# Edit index.html
git add index.html
git commit -m "fix: describe changes"
git push origin main
# Cloudflare Pages auto-deploys in ~30 seconds
```

### To Run Locally
```bash
# No dependencies — just open the file:
start index.html

# Or serve via Python:
python -m http.server 8080
open http://localhost:8080
```

---

## Verification Tests

All verified at deploy time (2026-08-06):

| Test | Expected | Result |
|:-----|:---------|:-------|
| Tree construction (p=2, d=3) | 15 nodes | ✅ 15 |
| Depth 3 leaf count | 8 (p^d) | ✅ [1,2,4,8] |
| Error subtree (node 2) | 7 nodes | ✅ 7 |
| Error subtree (node 3) | 3 nodes | ✅ 3 |
| Total redundancy | 14 | ✅ 14 |
| Canvas render | Non-zero dimensions | ✅ 940px |
| JS syntax | node --check | ✅ PASS |
| Production URL | HTTP 200, BTTree engine present | ✅ |

---

## Related Work

- **Paper**: [Ultrametric Code Spaces (DOI: 10.5281/zenodo.21817596)](https://doi.org/10.5281/zenodo.21817596)
- **QNFO Precedents**: p-adic stabilizer codes [10.5281/zenodo.20556327](https://doi.org/10.5281/zenodo.20556327), ultrametric tree QEC [10.5281/zenodo.21046993](https://doi.org/10.5281/zenodo.21046993)
- **Maity et al. no-go theorem**: [arXiv:2608.03944](https://arxiv.org/abs/2608.03944)

---

## License

QNFO Unified License Agreement (QNFO-ULA). Published as companion demo to DOI 10.5281/zenodo.21817596.
