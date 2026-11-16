# Build notes - Protein Design & Engineering

Repo2RLEnv pr_diff (v0.9.6), Harbor-format output. Domain: protein-design-engineering.

## Funnel

| Stage | Count |
|---|---|
| Domain candidates (deduped) | 352 |
| After active+star+noise filter | 124 |
| Final target list (cap unlimited) | 124 |
| Repos with >=1 task | 21 |
| Repos with 0 tasks | 86 |
| Total tasks (unlimited (no per-repo cap)) | 239 |

## Generation

- Shared task bank: each repo generated once (pr_diff, limit=3,
  max_files_per_pr=15, min_loc_changed=2), 4-way concurrency,
  rate-limit-aware with .done checkpoint resume.
- Per-domain consolidation: rank by reward_calibration.loc_changed desc,
  keep ALL qualifying tasks (no per-repo cap). Zero-yield repos are NOT fabricated.
- Overlap with the 3 published cancer editions is allowed (user decision);
  domains are independent selections, not a partition of one repo set.

## Zero-yield repos (informational, 86)

- hpcaitech/ColossalAI
- google-deepmind/alphafold3
- lucidrains/x-transformers
- graphistry/pygraphistry
- geohot/corona
- AGI-Edgerunners/LLM-Agents-Papers
- NVIDIA-NeMo/Nemotron
- color-js/color.js
- mvanhorn/printing-press-library
- tomohideshibata/BERT-related-papers
- allenai/scispacy
- stared/science-based-games-list
- Xtra-Computing/thundersvm
- DLTcollab/sse2neon
- Vincentqyw/cv-arxiv-daily
- jaydenseric/graphql-upload
- polyuiislab/infiAgent
- 52CV/CVPR-2024-Papers
- sqlite/sqlite-wasm
- lixus7/Time-Series-Works-Conferences
- sokrypton/ColabDesign
- jenetics/jenetics
- d-krupke/cpsat-primer
- generatebio/chroma
- mims-harvard/AutoScientists
- jdlc105/Must-read-papers-and-continuous-tracking-on-Graph-Neural-Network-GNN-progress
- esmnext/esmx
- RosettaCommons/RFD4-Proteina
- nanoexpress/nanoexpress
- NVIDIA-BioNeMo/Proteina-Complexa
- escalante-bio/mosaic
- wooorm/npm-esm-vs-cjs
- Julien-R44/hot-hook
- iambumblehead/esmock
- A4Bio/ProteinInvBench
- antfu/export-size
- aqlaboratory/genie
- mgechev/is-esm
- COLA-Laboratory/GraphFLA
- tinyhttp/malibu
- westlake-repl/Denovo-Pinal
- rohitsinghlab/raygun
- evo-design/protein-dpo
- richardshuai/fampnn
- patrickbryant1/RareFold
- ai4protein/VenusREM
- wooorm/import-meta-resolve
- ngruver/NOS
- bytedance/PXDesignBench
- MSDLLCpapers/ovo
- aqlaboratory/genie3
- AISciLab/FoldBack
- nodejs/loaders
- debbiemarkslab/SeqDesign
- ProteinDesignLab/caliby
- WaymentSteeleLab/Dyna-1
- OpenProteinAI/PoET
- Eikor/InstructPLM
- vanillaes/csv
- uhop/stream-chain
- NatLabRockies/EvoProtGrad
- ai4protein/ProtSSN
- polizzilab/LASErMPNN
- MinchaoFang/HalluDesign
- KrishnaswamyLab/ReLSO-Guided-Generative-Protein-Design-using-Regularized-Transformers
- meilerlab/HyperMPNN
- Wangchentong/Proteus
- uci-cbcl/esm-efficient
- AngxiaoYue/ReQFlow
- blt2114/MotifBench
- ProteinDesignLab/protein_seq_des
- poppinss/utils
- lifanchen-simm/transformerCPI2.0
- ChenyuWang-Monica/DRAKES
- dakpinaroglu/Frame2seq
- charlesxu90/ProteinMCP
- ProgressiveWebComponents/pwa-install
- westlake-repl/Evolla
- ramithuh/protein-design-interactive-slides
- cryoEM-CNIO/BinderFlow
- ZekunCheng/novoweave
- Genentech/walk-jump
- kumarshreshtha/protuna
- K-Dense-AI/drug-discovery-agent-skills
- SwiftSeal/resistify
- lucidrains/neural-plexer-pytorch

## Caveats

- Static validation only; real grading needs an LLM judge key + Docker.
- inline_dockerfile mode skips image build (no Docker daemon needed to publish).
- Provider secrets in upstream diffs were redacted before public release.
