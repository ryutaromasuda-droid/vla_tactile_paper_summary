# Learning Physical Interaction: A Survey of Tactile- and Force-aware Robot Learning

Note: text extracted with PyMuPDF (Read tool could not render pages); all 39 body pages plus the start of the references were read as text. Figure contents are inferred from text/captions only. The PDF is a survey, so "Method" = the TF-ART taxonomy and "Experiments" = what the survey reports about the surveyed corpus (it runs no experiments).

## 0. Paper Information
- Title: Learning Physical Interaction: A Survey of Tactile- and Force-aware Robot Learning (p.1)
- Authors: Shilin Shan (project lead), Chuhao Zhou, Ruize Wang, Xinyan Chen, Xiangyu Chen, Xinyu Zhou, Boyu Ma (equal contribution) and 30+ co-authors incl. Yanjie Ze, Haoran Geng, Tatsuya Harada, Harold Soh, Edward Johns, Danica Kragic, Jan Peters, Masayoshi Tomizuka, Jitendra Malik; corresponding author Jianfei Yang (NTU) (p.1)
- Affiliations (p.1): NTU, Stanford, UC Berkeley, MIT, NUS, Georgia Tech, U. Tokyo, ETH Zurich, Harvard, Imperial, KTH, TU Darmstadt.
- Venue/year: arXiv preprint, arXiv:2608.07558v1 [cs.RO], 2 Aug 2026 (p.1). Peer-reviewed venue: not stated.
- Paper URL: https://arxiv.org/abs/2608.07558 (derived from the arXiv ID on p.1)
- Project (webpage): https://lorenzo-0-0.github.io/tactile-force-survey/ (p.1)
- Code/curated list: https://github.com/NTUMARS/Awesome-Tactile-Force-aware-Robot-Learning (p.1)
- Summary (<=40 words): Survey of 53 tactile- and force-aware robot-learning papers (2024 to Apr 2026). Proposes TF-ART, a taxonomy of fusion, primary policy, action refinement, and compliant control phases. Finds tasks skewed to insertion and wiping; dynamic and long-horizon tasks under-explored.

## 1. Motivation
- Why it matters (p.1): in contact, task-critical information shifts from vision to interaction feedback. The object may be occluded or deformed, and success depends on where contact occurs, whether it is stable, and how large the force is.
  - Force sensing = global interaction (end-effector wrench, joint torque); tactile = local contact (pressure distribution, deformation, texture) (p.2).
- Gap in existing surveys (p.2-3): prior surveys are organized by sensor hardware, tactile perception, robot learning in general, contact-rich imitation learning, or foundation models. None uses the full multimodal policy-to-control pipeline as the organizing axis (Table 1, p.3).
- Central tension (p.2): large vision-language policies give semantic reasoning, but may not meet the low-latency, high-precision needs of local force-sensitive interaction. Hence multi-phase designs (high-level policy, refinement, low-level controller).
- Figure 1 (p.2): TF-ART overview. Phase 0 multimodal fusion, Phase 1 primary-action policy, Phase 2 refined-action policy, Phase 3 reactive robot control. Branches: input reconstruction, missing-modality prediction, explicit force control. Good "one picture" slide.
- Figure 2 (p.5): full TF-ART map with every one of the 53 papers' reference numbers placed on the modules. Dense; screenshot only as a backdrop.

## 2. Method (TF-ART taxonomy)
Terminology:
- Tactile-aware (p.4): uses contact measurements from camera-based tactile sensors, tactile pads/arrays, robot skins, or similar.
- Force-aware (p.4): uses or estimates force from wrist/EEF F/T sensors, joint-torque sensors, motor currents, or other proprioceptive signals.
- Inclusion rule (p.4): tactile/force must be a policy input, training signal, or predicted intermediate/output of a learned action-generation policy. Sensor-design, calibration, force-estimation-only, and pure classical-control papers are excluded.
- Search (p.4): Google Scholar, arXiv, IEEE Xplore, plus citation tracing; core corpus 2024 to Apr 2026; reference list last updated 21 Jul 2026; final corpus 53 papers.
- Multi-phase = explicit separation between primary action generation and later contact-aware refinement or force/compliance control (p.4).

Pipeline (Fig. 1 p.2, Fig. 2 p.5):
1. Phase 0, fusion and representation (Sec. 2, p.6-16)
   - Tactile formats (p.7-8): visual-tactile images, tactile matrices (taxels), contact points. Force formats (p.9): EEF wrench (6D), joint torque, tactile-based force estimates.
   - Non-contact modalities (p.9-10): language, vision (RGB, depth, point cloud), state (EEF pose, joint angle, object pose).
   - Representation learning (p.12-13): auxiliary reconstruction, masked autoencoding, VQ-based reconstruction, multimodal contrastive alignment, task-relevant supervision.
   - Fusion (p.14-15): FiLM injection, attention conditioning, Mixture-of-Experts, gated filtering. Fig. 3 (p.10) illustrates modalities and fusion structures.
   - Table 2 (p.11): per-paper modality usage, fusion method, representation learning.
2. Phase 1, primary action policy (Sec. 3, p.17-22)
   - Five architectures (p.17): regression (MLP), RL, diffusion/flow matching, ACT, VLA. Fig. 4 (p.17), Table 3 (p.20).
   - Outputs (p.19-21): actions (EEF, joint, delta), reference force, control stiffness, auxiliary predictions. Sensory prediction for missing modalities (p.21).
   - VLA note (p.19): pi0, ForceVLA, Tactile-VLA decode VLM embeddings into continuous actions via diffusion/flow/regression heads. Strong open-vocabulary generalization but high training and inference cost; bridging to high-frequency control is "a central architectural challenge".
   - Single-phase challenge (p.22): vision-language dominance adds latency; contact-intensive policies benefit from low-dimensional force/tactile and compact models (DP, ACT).
3. Phase 2, modality-discriminative action refinement (Sec. 4, p.22-25)
   - Some raw modalities (tactile, F/T, proprioception, local vision) bypass Phase 1 and go straight to the refiner ("discriminative modality injection", p.23).
   - Refiner types (p.24): RL residual (UPPFC, FCLM, ViTaL), lightweight ACT (RDP, 20-30 Hz), diffusion (Force-Policy, VLA-Touch), shallow regression MLP (OmniVTA), rule/model-based (FoAR, TacDiffusion, KineDex, TACT, ForceSight). Outputs are mostly incremental/residual actions. Table 4 (p.25), Fig. 5 (p.23).
4. Phase 3, robot-end control (Sec. 5, p.25-30)
   - PD/PID, impedance, admittance, hybrid force/position (p.26-28); Table 5 (p.29) lists controller, platform, end-effector per paper. Fig. 6 (p.26).
   - Practical (p.30): many methods still use position control; impedance control is the most widely used compliant method (typically Franka, KUKA, ROKAE); hybrid force/position typically on Flexiv Rizon. Poorly tuned gains still cause large forces and overshoot.
5. Tasks and sensors (Sec. 6, p.31-35).

Differences from existing surveys (p.3): the unit of analysis is the information and control flow, not sensor type, task, or learning paradigm. It identifies where and for what purpose force/tactile enters the system.

Key equations (all generic, not the authors' own method):
- FiLM (p.14, Eq. 7): $\tilde A_t=\gamma_m\odot A_t+\beta_m$.
- Gated filtering (p.15, Eq. 10): $\tilde F_m=\sigma(G_m(s_t))\odot F_m$, with $s_t$ = e.g. EEF pose, contact state, or phase. Described as a "learned sensory switch".
- MoE (p.15, Eq. 9): $\tilde f_j=\sum_{i\in S_j} r_{j,i}E_i(f_j)$.
- Potential directions (p.16, Eq. 11-16): denoising-stage modality composition, balanced multimodal fusion (gradient modulation $\rho_m$), foresight modality imagination (world model predicts future tactile latents; OmniVTA cited as an example).

## 3. Experiments (what the survey reports; no new experiments)
- Corpus (p.4, p.31): 53 papers, 21 manipulation tasks in 4 categories (p.31): precision contact, deformable/surface contact, dynamic/multi-contact, household/long-horizon.
  - Which phase each category stresses (p.31): precision contact -> Phase 2/3; deformable -> Phase 0/3; dynamic -> Phase 2/3 coupling; household/long-horizon -> Phase 1.
- Task distribution (p.33, Fig. 7): peg-in-hole insertion 26 papers, wiping 18, pick-and-place 13; together nearly half of all task-method pairs. Tasks 6-11 papers each: A3, D1, B5, B1, C3 (labels as in Fig. 7; task names for these not read).
- Sensors (p.33-35, Table 7 p.34): 18/53 (34%) wrist F/T, 9/53 (17%) joint torque (2 use both), 27/53 (51%) tactile, 4 pair a force-domain sensor with a tactile sensor (RDP, TACT, TaF-VLA, MoDE-VLA), 5 estimate force from proprioception or use vision only.
  - Vision-based tactile: 15 methods; 7 use GelSight family (GelSight, Mini, GelStereo; one also MC-Tac); others DIGIT, STS, SharpaWave-integrated, Xense, custom (p.35).
  - Taxel arrays (resistive, capacitive, barometric, magnetic) have higher sampling rates and fit curved surfaces; vibration/acoustic sensing (e.g., BioTac) is not yet common in the corpus (p.35).
- Takeaway (p.36-39): the field is far from transferable physical-interaction intelligence. Open problems and future directions:
  - No shared pretrained backbones for force/tactile; heavy cross-sensor and cross-embodiment variation (p.36).
  - Need a generalizable phase-encoding framework (non-contact, pre-contact, in-contact) (p.37).
  - Formalize slow-reasoning plus fast-reactive division of labor; skill experts (p.37).
  - Policies that predict reference force/stiffness are tied to platforms; want compliant-behavior-mimicking policies for position-controlled low-cost robots (p.37).
  - Benchmarks should measure force safety, contact stability, recovery, unseen materials, sensor failure (p.38).
  - Broader: simulation and data scaling, contact-informed world action models, touch/force as intrinsic critics, mechanical compliance as a design variable (p.38-39).
- Limitations of the field (p.35): most evaluations are single-stage tasks on fixed platforms in controlled settings; dynamic multi-contact, long-horizon, and cross-platform generalization are under-explored; no standardized evaluation protocols for tactile/force foundation models.
- Limitations of the survey itself: not stated as a dedicated section; scope is bounded by the inclusion rule (learned action-generation policies only; p.4).
- Slide-worthy figures: Fig. 1 (p.2) overall framework; Fig. 3 (p.10) fusion structures; Fig. 7 (p.33) task histogram; Table 7 (p.34) sensor comparison; Fig. 2 (p.5) only as a dense backdrop.


---

# OmniVTA: Visuo-Tactile World Modeling for Contact-Rich Robotic Manipulation

Note: text extracted with PyMuPDF (21 pages, main + references; no separate appendix); figure contents inferred from captions/text only.

## 0. Paper Information
- Title: OmniVTA: Visuo-Tactile World Modeling for Contact-Rich Robotic Manipulation (p.1)
- Authors: Yuhang Zheng*, Songen Gu*, Yupeng Zheng†, Weize Li, Yujie Zang, Shuai Tian, Xiang Li, Ce Hao, Chen Gao, Si Liu, Haoran Li, Yilun Chen, Shuicheng Yan†, Wenchao Ding† (*equal, †corresponding) (p.1)
- Affiliations (p.1): NUS, CASIA, Fudan, TARS Robotics, Tsinghua, Zhongguancun Academy, Beihang.
- Venue/year: not stated (arXiv:2603.19201v3 [cs.RO], 10 Aug 2026, p.1).
- arXiv: https://arxiv.org/abs/2603.19201 (from the ID on p.1)
- Project page: not stated.
- Code/data: not released as URL; abstract says "all data, models, and code will be made publicly available" (p.1).
- Summary (<=40 words): OmniVTA pairs a 21.9K-trajectory visuo-tactile dataset (OmniViTac) with a slow-fast policy. A visuo-tactile world model predicts future tactile latents; a 60 Hz reflexive controller corrects deviations. It beats RDP and other baselines on six contact-rich task types.

## 1. Motivation
- Contact-rich tasks (wiping, assembly) need contact force, friction and state-transition perception that vision alone cannot give (p.1).
- Limits of existing work (p.1-2):
  - Data: public visuo-tactile-action datasets are small and narrow in tasks/interaction patterns (Table I, p.5).
  - Methods: tactile is only a passive auxiliary observation (contact recognition, occlusion compensation), not used to model contact dynamics.
  - Control: action chunks are executed open loop, so slip, misalignment and disturbances are hard to react to.
- Inspiration (p.2): human sensorimotor control = feedforward prediction of contact + fast tactile feedback correction.
- Figure 1 (p.1): overview of the whole system: left, OmniViTac dataset; center, OmniVTA world-model-based framework (tactile representation, predictive modeling, adaptive fusion, reflexive control); right, real-world results vs prior methods. Fairly dense; usable as a one-slide overview.
- Contributions (p.2): (1) OmniViTac dataset + two tactile-signal properties (spatial locality, contact-driven dynamics); (2) OmniVTA framework; (3) real-robot gains in robustness and generalization.

## 2. Method
Terminology:
- OmniViTac: dataset, 21,879 trajectories, 86 tasks, 100+ objects (the Table I row says 126 objects), six patterns: wiping, peeling, cutting, grasping, assembly, in-hand adjustment (p.2, p.4-6).
- TacUMI: handheld UMI-style visuo-tactile data collection device (FastUMI-inspired), same parallel-jaw gripper as the xArm-7 (p.4).
- Tactile input: 3D marker displacement field (H x W x 3) of optical tactile sensors, not raw gel images (p.8).
- VTWM: Visuo-Tactile World Model. AFP: Adaptive Visuo-Tactile Fusion Policy. LTD: Latent Tactile Differential encoder. RLTC: Reflexive Latent Tactile Controller (p.7-8).

Pipeline (Fig. 5, p.8: slow-fast hierarchy):
1. TactileVAE (Fig. 6, p.9): causal 3D-conv VAE over (x,y,z) marker displacements, spatio-temporal compression (temporal factor M=4, p.11), latent map H/s x W/s x C. Implicit neural decoder reconstructs a continuous deformation field. 1.96M params (p.13).
2. VTWM (Fig. 7a, p.10): two-stream spatial-temporal diffusion transformer, one stream for visual latents (SD-VAE), one for tactile latents (TactileVAE). Conditioned on past frames plus a shared multimodal conditioner (vision, tactile, history actions projected to the 2D image plane). Dynamic-aware weighted loss emphasizes high-frequency tactile changes. At inference only tactile (not visual) is predicted, for speed (p.8).
3. AFP (Fig. 7b, p.10-11): LTD encoder builds f_t = concat(current, predicted, predicted - current). Contact-probability head (BCE) feeds a gating net that outputs per-channel weights W_t + W_v = 1. Fused features + proprioception condition a CNN diffusion policy (FiLM). Outputs coarse action chunk A_c (6 actions at 15 Hz, interpolated to 60 Hz, p.12).
4. RLTC (Fig. 8, p.11): 3-layer MLP takes the current tactile latent, chunk-aligned predicted tactile latent (15 Hz upsampled 4x to 60 Hz) via LTD, and recent delta actions; outputs a residual action a_r at 60 Hz. Trained on expert recovery segments (target = expert action minus slow-policy action) and normal-contact segments (target = 0) (p.11-12). Final action = slow action + scaled fast residual (p.8).
- Training: four stages (TactileVAE, VTWM, AFP, RLTC) (p.12). Total 600.52M params (Table III, p.13).

How it differs from existing methods:
- Tactile is used to predict future contact (world model) rather than only observed passively (p.2, p.8).
- Closed-loop 60 Hz tactile correction on top of chunked planning; the authors contrast with RDP's slow-fast design, saying their controller regulates toward predicted tactile targets (p.14).
- Gating by predicted contact state; the gate needs no visual input (p.11).

Key equations:
- Implicit decoder (Eq.1, p.8): d(x) = D_theta(gamma(x), Phi(z_t, x)).
- Tactile fusion (Eq.9, p.10): f_t = concat(f_t^c, f_t^p, f_t^p - f_t^c).
- Gated fusion (Eq.10, p.11): f_vt = concat(W_v * f_v, W_t * f~_t).
- World-model loss (Eq.8, p.10): L_VTWM = L_diffusion + lambda1 L_dyn + lambda2 L_amp, with dynamic weight w_dyn = resize(clip(||X_{i+1} - X_i||)) (Eq.4, p.9).
- Controller loss (Eq.14, p.12): L_RLTC = ||a_r - a_r_hat||^2.

Pseudocode (my paraphrase, not in the paper):
```
slow loop (15 Hz chunk, 230 ms inference):
  z_pred = VTWM(past image, tactile, hist actions)   # tactile only
  f_t = LTD(current tactile, z_pred); w = gate(contact_prob(f_t))
  A_c = DiffusionPolicy(w*[vision, tactile], proprio)
fast loop (60 Hz, 3.5 ms):
  a_r = RLTC(tactile_now, z_pred_upsampled, delta_actions)
  execute a_c[t] + k * a_r
```

## 3. Experiments
Dataset analysis (p.4-7): Fig. 2 (p.3) data collection platform and scale comparison; Fig. 3 (p.5) the six patterns; Fig. 4 (p.7) statistics (contact area, force, active ratio, t-SNE). Grasping is the largest subset (9,200 trajs); Effective Contact Ratio: Adjustment 0.67, Wiping 0.49, Peeling 0.41, Cutting 0.27 (p.6).

Hardware (p.4-5, p.13):
- Data collection: UFACTORY xArm-7 (kinesthetic teaching, GELLO teleop) and TacUMI; parallel-jaw gripper; RealSense D435 wrist + D435/L515 third-person, 30 Hz.
- Tactile sensors supported: Xense Quark N1 (3D displacement 35x20 at 60 Hz), Daimon Tac-WL, Tac3D-A1, GelSight Mini (25 Hz).
- Real-robot evaluation: xArm7, two-finger gripper, two fingertip Xense sensors (60 Hz tactile), wrist D435 at 15 Hz (p.13).
- Policy training data: 5-6 objects per task, 150 trajectories per object (Table II, p.12). Inference on RTX 4090D: slow policy 230 ms (480 ms with visual generation), fast policy 3.5 ms (Table X, p.17).

Baselines (p.13): DP, DP+tactile (PCA features), KineDex, ForceMimic, RDP, OmniVTA w/o RLTC. For world-model comparison: UVA, ForceMimic/KineDex-style joint diffusion, exUMI. TactileVAE baselines: PCA, PointNet-AE.

Tasks / settings (p.13): six tasks (wipe, peel, cut, assembly, grasp, adjustment); object diversity (O, 10 trials per object), generalization (G: unseen heights for wipe/peel/assembly/adjustment, unseen knife for cut), perturbation robustness (P: object displaced vertically during contact).

Key numbers (Table IV, p.14; success rate, O / G / P):
- Wipe: OmniVTA 0.80/0.58/0.60 vs RDP 0.50/0.38/0.42, DP 0.12/0.05/0.
- Peel: 0.55/0.48/0.63 vs RDP 0.48/0.36/0.45.
- Cut: 0.85/0.83/0.60 vs RDP 0.65/0.50/0.43.
- Assembly: 0.60/0.50/0.40 vs RDP 0.60/0.50/0.35 (O and G tie with RDP).
- Grasp (O only): 0.90 vs RDP 0.88. Adjustment (O/G): 0.65/0.65 vs RDP 0.50/0.50. (Column mapping read from flattened text; verify against Table IV p.14 before quoting.)
- OmniVTA w/o RLTC is below the full model in nearly all cells, e.g. wipe P 0.25 vs 0.60 (p.14).
- Contact deformation (p.14): OmniVTA avg tangential deformation 0.35 (max 0.72) vs RDP 0.56 (max 1.1); the paper says RDP often damages sensors.
- TactileVAE reconstruction beats PCA and PointNet-AE in all six categories (Table V, p.15), e.g. Wipe L2 0.038 vs 0.059/0.091.
- World model beats UVA, exUMI, KineDex, ForceMimic in tactile prediction L2/cos (Table VII, p.15-16); e.g. wipe L2avg 0.059, cos 0.93.
- Ablation (Table IX, p.17; wipe/peel avg SR): no tactile prediction 0.09; predicted tactile len 6: 0.40; + LTD 0.47; + gating 0.53; + generated visual features 0.54 (no meaningful gain, and halves frequency). Gating gives about +7% over concatenation (p.18).
- Prediction quality matters: policy success falls as world-model accuracy drops to 80/60/40/20% of best (Fig. 17, p.19).

Takeaway (authors): predictive visuo-tactile modeling plus high-frequency tactile reflex gives better success, unseen-height/tool generalization and perturbation recovery (p.14, p.19).

Limitations: no dedicated limitations section found. Observable from the paper: real-robot success rates are modest in several cells (e.g. 0.40-0.60 for peel/assembly in some settings, Table IV); only Xense sensors and a single xArm7 + parallel gripper used for real-robot evaluation (p.13); 10 trials per object; dataset analysis and policy training use a subset (5-6 objects per task, Table II).

Figures worth screenshotting: Fig. 1 (p.1, overview), Fig. 5 (p.8, slow-fast system), Fig. 7 (p.10, world model + fusion), Fig. 8 (p.11, RLTC), Fig. 2 (p.3, dataset/platform), Fig. 3 (p.5, six patterns), Table I (p.5, dataset comparison), Table IV (p.14), Fig. 10 (p.14, real-robot rollouts), Fig. 12 (p.16, predicted vs GT tactile), Fig. 14 (p.18, gating vs contact), Fig. 16 (p.19, recovery time).


---

# OmniVTLA: Vision-Tactile-Language-Action Models with Semantic-Aligned Tactile Sensing

Note: text extracted with PyMuPDF (17 pages incl. appendix); figure contents inferred from captions/text only.

## 0. Paper Information
- Title: OmniVTLA: Vision-Tactile-Language-Action Models with Semantic-Aligned Tactile Sensing (p.1)
- Authors: Zhengxue Cheng (corresponding), Yiqian Zhang, Anni Tang, Keyu Wang, Wenkang Zhang, Haoyu Li, Hengdi Zhang, Li Song (p.1). Affiliations: Shanghai Jiao Tong University; Paxini Tech.
- Venue/year: not stated (arXiv:2508.08706v3 [cs.RO], 15 Jun 2026; "Date: June 16, 2026", p.1).
- arXiv: https://arxiv.org/abs/2508.08706 (from the ID on p.1)
- Project / dataset page: https://readerek.github.io/Objtac.github.io/ (p.1). Dataset: https://omnisharingdb.paxini.com/ (p.1).
- Code URL: not stated.
- Summary (<=40 words): OmniVTLA extends pi0 with a dual-encoder tactile path (pretrained ViT + contrastively trained semantic-aligned SA-ViT) trained on the ObjTac dataset (135K tri-modal samples). It lifts gripper pick-and-place success to 96.9% and peg insertion to 83.3% over pi0.

## 1. Motivation
- VLAs rely on vision and language; difficulty of acquiring tactile data means they overlook touch and fail in contact-rich tasks (p.1).
- Tactile pre-training work (AnyTouch, etc.) focuses on perception tasks, not on manipulation or tactile change during interaction (p.1).
- Existing VTLA attempts (VTLA, Tactile-VLA) treat tactile as low-level signals without semantic alignment to vision/language (p.1, Table 1 p.3). Tactile-encoder design and cross-modal alignment are "rarely studied" (Fig. 1 caption, p.2).
- Figure 1 (p.2): left, a vanilla VLA where the image encoder (CLIP/SigLIP) is contrastively aligned with text; right, a VTLA with a tactile tokenizer and a question mark on whether tactile is semantically aligned. Simple diagram; good for a motivation slide.
- Table 1 (p.3): comparison of VA / VTA / VLA / TLA / VTLA models by input modalities and "Semantic-Aligned"; only OmniVTLA is V+T+L with semantic alignment.

## 2. Method
Terminology:
- VTLA: vision-tactile-language-action model.
- SA-ViT: semantically-aligned tactile ViT, trained with cross-modal contrastive learning across tactile, vision, language (p.3-6).
- ObjTac: the authors' tactile-vision-text dataset: 56 objects, 10 material categories, 135K tri-modal samples (p.2, p.5-6).
- Paxini Gen2: magnetic (Hall-effect) tactile sensor giving force fields computed on-board (p.5).

Pipeline (Fig. 2, p.4):
1. Base: built on pi0 (p.4). PaliGemma tokenizer for language; SigLIP (400M) for third-view and wrist images (224x224, 256 tokens each); Gemma-2.6B backbone [as written in the paper]; flow-matching action head (p.4-5).
2. Tactile data: normalized to int8, multi-sensor inputs stitched into one image, resized to 224x224, encoded by a ViT-like encoder to 256 tokens (p.5).
3. Dual-encoder tactile path: one pretrained ViT (inherits visual semantics) plus SA-ViT (trained for tactile-vision-language alignment); both feed the backbone (p.3, Fig. 2).
4. SA-ViT training: AnyTouch second-stage pipeline on existing datasets (Touch and Go, SSVTP, ObjectFolder) plus ObjTac; alignment loss over V-L, V-T, T-L pairs with weights alpha, plus cross-sensor matching BCE (Eq.3, p.6).
5. Actions: two-finger gripper = 10 tokens (3 rel. position, 6 rel. angle, 1 gripper); four-finger hand = 25 tokens (3 + 6 + 16 absolute joints) (p.5).

How it differs: tactile is aligned semantically with vision and language and fed through a dual encoder, rather than a single tactile encoder trained from scratch or taken from a vision encoder (p.2-3). Parameter-matched controls (VTLA-Pre-Pre) are used to isolate the effect (p.7).

Key equations:
- VLA (Eq.1, p.4): A_t ~ M_VLA(f_phi(I_t^i), l_t).
- VTLA (Eq.2, p.4): A_t ~ M_VTLA(f_phi^I(I_t^i), f_theta^T(T_t^j), l_t).
- Alignment loss (Eq.3, p.6): L_align = alpha_VL (L_V->L + L_L->V)/2 + alpha_VT (...)/2 + alpha_TL (...)/2.

Pseudocode: not in paper (architecture description only).

## 3. Experiments
Tactile encoder evaluation (Table 2, p.5): material/roughness/hardness classification on Touch and Go and on ObjTac. SA-ViT on the authors' data: material 70.44, roughness 82.21, hardness 93.91, much higher than CLIP/SigLIP/Tac-ViT/AnyTouch on material (e.g. AnyTouch 40.21); on Touch and Go SA-ViT 74.90 / 85.46 / 92.10, slightly below AnyTouch (79.39 material).

Baselines (p.6-7): pi0 (VLA), Diffusion Policy (VA), and encoder variants of VTLA: VTLA-FS (from scratch), VTLA-Pre (initialized from pretrained vision encoder), VTLA-SA (SA-ViT only), VTLA-Pre-Pre (SigLIP + CLIP, same parameter count), VTLA-Pre-Tac (SigLIP + Tac-ViT w/o alignment), OmniVTLA (SigLIP + SA-ViT).

Hardware (p.6-7, Fig. 4 p.7): UR5 arm; jaw gripper with two tactile sensors and wrist camera; dexterous hand with 11 tactile sensors and wrist camera; a base camera. Tactile sensor: Paxini Gen2 (p.5). Fig. 9 mentions DexH13 hand (p.16).

Tasks (p.6-7): 
- Pick-and-place: gripper, 4 objects (short can, square coffee bottle, gum tin, milk carton), 32 rollouts; dexterous hand, 2 objects, 16 rollouts; 40 teleoperated demos per object at 30 Hz; unseen: plastic bottle, square bottle, sponge, towel; max 1500 steps.
- Peg insertion: square, triangle (54 demos), round held out as unseen; clearances 0.9/1.2/1.8 mm; 10 trials each.
- Training (Table 10, p.15): A100 80GB, fine-tune pi0 30K steps; DP trained from scratch 200K steps.

Key numbers:
- Gripper pick-and-place (Table 3, p.8): pi0 75.0% SR, 657 steps; OmniVTLA 96.9% SR, 498 steps (-24.2% time); VTLA-SA 87.5%, 484 steps (lowest CT).
- Dexterous hand (Table 4, p.8): pi0 93.8% / 343 steps; OmniVTLA 100% / 322 steps; unseen objects 100% vs 87.5%.
- DP with vs without tactile (Table 5, p.8): 59.4% -> 78.1% SR; 851 -> 682 steps.
- Peg insertion (Table 6, p.9): pi0 50.0%; OmniVTLA 83.3% (+33.3), including unseen round 90% vs 30%; VTLA-Pre-Pre 53.3%, VTLA-Pre-Tac 43.3% (so the gain is attributed to the semantic-aligned encoder, not parameter count). Completion time: OmniVTLA 802 vs pi0 816; VTLA-SA 678 is fastest.
- Soft objects (Table 8, p.10): OmniVTLA 81.3% vs pi0 50.0%; VTLA-FS/Pre get 93.8% but via tight gripper closure; OmniVTLA and VTLA-SA have the smallest max gripper width (0.583, 0.558), meaning gentler grasps.
- Smoothness (Table 7, p.9): VTLA-SA best; OmniVTLA end-effector 2.65e-6 vs pi0 2.70e-6, gripper 2.26e-3 vs 2.52e-3 (small gains).
- Offline MSE (Fig. 5, p.8): VTLA-Pre-Tac lowest, OmniVTLA second lowest.

Takeaway (authors): tactile helps both pi0 and DP; semantic-aligned dual-encoder is the best design under matched parameters; fewer retry loops and faster completion (p.9-10).

Limitations (p.12): training data included repeated attempts for both tasks, but single-attempt data sufficed for pick-and-place and only contact-rich tasks needed multiple attempts; no improvement in messy clutter where success depends on visual navigation; need for more efficient tactile representations and temporally dynamic fusion. Observations from tables: OmniVTLA is not best on every metric (VTLA-SA wins several CT/smoothness cells; VTLA-Pre-Tac wins offline MSE); small trial counts (8-32 rollouts, 10 for peg insertion).

Figures worth screenshotting: Fig. 1 (p.2, motivation), Fig. 2 (p.4, architecture), Fig. 3 (p.5, ObjTac samples), Table 2 (p.5), Fig. 4 (p.7, hardware and objects), Table 3 and Table 6 (p.8-9), Fig. 6 and Fig. 7 (p.10-11, stage-by-stage VLA vs OmniVTLA), Fig. 8 (p.11, gripper width on soft objects).


---

# TA-VLA: Elucidating the Design Space of Torque-aware Vision-Language-Action Models

Note: text extracted with PyMuPDF (19 pages incl. references and appendix); figure contents inferred from captions/text only. Important: this paper uses joint torque (motor current), not a tactile sensor.

## 0. Paper Information
- Title: TA-VLA: Elucidating the Design Space of Torque-aware Vision-Language-Action Models (p.1)
- Authors: Zongzheng Zhang*, Haobo Xu*, Zhuo Yang*, Chenghao Yue, Zehao Lin, Huan-ang Gao, Ziwei Wang, Hao Zhao† (*equal, †corresponding) (p.1). Affiliations: BAAI; AIR, Tsinghua; NTU.
- Venue/year: CoRL 2025 (9th Conference on Robot Learning, Seoul; p.1). arXiv:2509.07962v1, 9 Sep 2025.
- arXiv: https://arxiv.org/abs/2509.07962 (from the ID on p.1)
- Project page: https://zzongzheng0918.github.io/Torque-Aware-VLA.github.io/ (p.1)
- Code URL: not stated.
- Summary (<=40 words): TA-VLA systematically studies how to add joint torque to pi0. Findings: put torque in the decoder, compress torque history into one token, and predict future torque as an auxiliary output. Contact-rich success rises from about 23% to 86% (pi0 baseline).

## 1. Motivation
- Force cues such as joint torque indicate whether a task succeeded and enable closed-loop control, but VLAs lack the ability to use such feedback (p.1).
- Joint torque reflects end-effector contact without external force sensors (p.1); prior force/torque work mostly needs extra 6D wrench sensors (cost, harsh conditions) and trains from scratch rather than using pretrained VLAs (p.3). FACTR is cited as needing a complex training pipeline (p.3).
- Challenge: torque is a proprioceptive signal structurally unlike image/text, and varies over time during contact (p.1).
- Figure 1 (p.2): (a) torque profiles of a 7-DoF arm during charger insertion: no contact (flat), failed insertion (small fluctuation), successful insertion (large spikes); (b) joint mapping; (c) design space of torque usage along three axes: when (immediate / history / future), where (encoder / decoder), how (single token / multi-token). Very informative; use on a motivation slide.

## 2. Method
Terminology:
- Encoder (conditioning encoder): VLM part processing images and language (in pi0, PaliGemma). Decoder (denoising decoder): action expert that denoises the action chunk (p.4).
- Torque = joint effort of all 7 joints, derived from motor current: tau = k_t * i (p.7, p.17).
- HSIC: Hilbert-Schmidt Independence Criterion, used to measure dependence between modality features (p.5).

Pipeline: study done on pi0 (also RDT in cross-model test) in three steps.
1. Sec. 3 (p.3-4): theory: tau_measured = tau_model + J^T(q) F_ext, so deviation of joint torques from the nominal model gives the external wrench.
2. Where to embed (Sec. 4.1, Fig. 2, p.4-5): Enc (torque token added to encoder conditioning), DePre (torque written into zero-padded dims of the state token), DePost (torque adapter token prepended to action-expert state inputs). Adapter = MLP (14-dim input).
3. How to use history (Sec. 4.2, Fig. 4, p.6): frame-wise tokens (H tokens) vs one aggregated token, in encoder vs decoder. History = 10 frames uniformly sampled from the past 2 s, flattened to a 140-dim vector for the 1-token case (p.15).
4. Torque as an objective (Sec. 5, Fig. 5, p.6): action-torque joint diffusion. One linear output layer predicts concatenated action and future-torque chunks (H = 50); L_joint = L_action + beta * L_torque.
- Final variants: pi0+obs (DePost-1 token, torque observation), pi0+obj (auxiliary torque prediction), pi0+obs+obj (both) (p.7).

How it differs: rather than proposing a new architecture, it provides a design-space study and evidence for why: torque aligns with joint angles (HSIC, Fig. 3 p.5), decoder is more sensitive to input noise, and extra history tokens disturb the decoder's learned input pattern (Tables 2 and 4, p.5-6).

Key equations:
- Dynamics with contact (Eq.1, p.3): M(q)q'' + C(q,q')q' + G(q) = tau_cmd + tau_ext.
- Torque from wrench (Eq.2, p.4): tau_ext = J^T(q) F_ext.
- Joint loss (p.6): L_joint = L_action + beta * L_torque, with noisy input Z^alpha = alpha Z + (1 - alpha) eps, Z = [A; T].

Pseudocode: not in paper.

## 3. Experiments
Hardware (p.7): Cobot Magic ALOHA, dual-arm, 7 DoF per arm; three RealSense D435 (two wrist, one front). No tactile sensor; torque from motor current (kt * i). Cross-embodiment: ROKAE SR arm (p.7, p.17). Inference on an RTX 4090; training 4x L20 (p.16).

Baselines (p.7): ACT, RDT, pi0 (all fine-tuned from public weights on the authors' data). 400 teleoperated demonstrations per task (p.17); 20 trials per task.

Tasks: 5 contact-rich (Button Pushing, Charger Plugging, USB Plugging, Socket Unplugging, Door Handle Turning) and 5 regular (Bottle Pick and Place, Liquid Pouring, Stacking Cubes, Push-to-Position, Opening a Drawer) (Table 5, p.8).

Key numbers (success out of 20):
- Where to embed (Table 1, p.5): Button Pushing pi0 5, Enc 7, DePre 8, DePost 10; Charger Plugging pi0 0, Enc 8, DePre 11, DePost 12.
- History (Table 3, p.5): Button Pushing pi0 5, Enc-1 1, Enc-H 4, Dec-1 15, Dec-H 9; Charger Plugging pi0 0, Enc-1 3, Enc-H 6, Dec-1 16, Dec-H 7.
- Noise tests (Tables 2, 4, p.5): noising the decoder state token hurts more (Button Pushing 5 -> 0) than noising encoder tokens (5 -> 4); extra noise tokens in the decoder: 5 -> 2.
- Full results (Table 5, p.8), contact-rich: pi0 5/0/0/16/2; pi0+obs 15/16/15/19/13; pi0+obj 11/10/12/19/12; pi0+obs+obj 18/17/17/19/15; ACT 2/0/0/12/0; RDT 4/1/0/10/0. Computed averages (mine): pi0 23%, pi0+obs 78%, pi0+obj 64%, pi0+obs+obj 86%.
- Regular tasks (Table 5): pi0 17/16/17/16/19 (85% avg, mine); pi0+obs+obj 19/17/17/18/18 (89% avg, mine), i.e. small gains.
- Cross-model (Table 6, p.7): RDT -> RDT+obs+obj: Button Pushing 4 -> 16, Charger Plugging 1 -> 15, Bottle Pick and Place 17 -> 19.
- Efficiency (A.8, p.18): inference 90.70 ms (pi0) vs 93.96 ms (pi0+obs+obj).
- Ablations: beta = 0.1 best for obs+obj (18/20; Table 9, p.18); MLP aggregation beats RNN and attention (15/20 vs 7 and 13 for pi0+obs; Table 10, p.18). Predicted torque closely tracks ground truth (Fig. 6, p.7).
- Qualitative (Fig. 7, p.8): on a failed attempt (misalignment/slip) torque change triggers retry and second attempt succeeds. Cross-embodiment charger insertion with ROKAE SR (Fig. 8, p.8; 200 demos, 50K steps, p.17), also adapts to a slow-charging port.

Takeaway (authors): decoder-side, single-token torque history plus action-torque joint prediction give the largest gain on contact-rich tasks and generalize across pi0/RDT and embodiments (p.8).

Limitations (p.9): depends on torque estimated from motor currents, which can suffer from calibration, noise, thermal drift; unclear how it scales to other physical modalities (tactile, temperature) under shared token budgets; needs evaluation in more diverse real-world settings. Also (observations): all real-robot, 20 trials per cell, tasks collected by authors; baselines pi0 gets 0/20 on three tasks, so the relative gains are large partly from a very low baseline.

Figures worth screenshotting: Fig. 1 (p.2, torque response and design space), Fig. 2 (p.4, Enc/DePre/DePost), Fig. 3 (p.5, HSIC alignment), Table 1 and Table 3 (p.5), Fig. 4 (p.6, history architectures), Fig. 5 (p.6, action-torque diffusion), Fig. 6 (p.7, torque prediction), Table 5 (p.8), Fig. 7 (p.8, retry behavior).


---

# CRAFT: Adapting VLA Models to Contact-rich Manipulation via Force-aware Curriculum Fine-tuning

Note: text extracted with PyMuPDF (8 pages incl. references); figure contents inferred from captions and text only. Bar values in Fig. 4 are not in the text.

## 0. Paper Information
- Title: CRAFT: Adapting VLA Models to Contact-rich Manipulation via Force-aware Curriculum Fine-tuning (p.1)
- Authors: Yike Zhang, Yaonan Wang, Xinxin Sun, Kaizhen Huang, Zhiyuan Xu, Junjie Ji, Zhengping Che, Jian Tang, Jingtao Sun (corresponding) (p.1)
- Affiliations (p.1): Hunan University (National Engineering Research Center of Robot Visual Perception and Control Technology); Beijing Innovation Center of Humanoid Robotics (X-Humanoid); National University of Singapore.
- Venue/year: not stated (arXiv preprint, 2026). arXiv:2602.12532v1 [cs.RO], 13 Feb 2026 (p.1)
- Paper URL: https://arxiv.org/abs/2602.12532 (from the arXiv ID on p.1)
- Project URL: not stated
- Code URL: not stated
- Summary (<=40 words): CRAFT adds a variational information bottleneck after the VLA's vision-language embeddings, with a decaying KL weight. The policy first relies on joint torque, then regains vision and language. It lifts pi0 and RDT success on five contact-rich tasks.

## 1. Motivation
- VLAs (RT-1/2, SayCan, OpenVLA, RDT, pi0) generalize well but struggle with contact-rich tasks: precise insertion, deformable-object rolling, continuous wiping (p.1).
- Two stated causes (p.1):
  - High-entropy vision and language inputs dominate and cannot capture subtle contact interaction.
  - No explicit force or tactile feedback, so resistance and slip are invisible.
- Existing methods (p.1-2): RDP (slow-fast visual-tactile policy), VLA-Touch (dual-level tactile feedback), ForceVLA (force-aware MoE). The paper says they remain limited by modality entropy imbalance and lack a unified mechanism to regulate multimodal contributions.
- Closest work, FACTR (p.2): curriculum that blurs visual input with decreasing intensity. CRAFT instead uses the Information Bottleneck principle instead of a heuristic degradation schedule.
- Naive concatenation of force with vision/language leaves the force modality underused (p.3).
- Figure 1 (p.1) is NOT a motivation figure: it shows the homologous leader-follower teleoperation system (leader arm, follower Franka arm, force feedback, two cameras). Fig. 2 (p.4) is the method overview and is the better slide figure.

## 2. Method
Terminology:
- Force signal: joint torque tau used as the proprioception input instead of joint position q (p.4). No external F/T or tactile sensor is added (p.3).
- VIB (variational information bottleneck): compresses embeddings F into a stochastic latent F^c, with a KL penalty to a standard normal prior (p.3-4).
- Curriculum: strong VIB weight first (force-first), exponentially decayed later (p.5).
- Homologous leader-follower teleoperation: low-cost servo leader arm, Franka Emika Panda follower, force feedback to the operator (p.3).

Pipeline (Fig. 2, p.4):
1. Collect demos with the leader-follower system. Synchronized: wrist camera, third-person camera, proprioception, joint torque, actions, language (p.3).
2. Take a pretrained VLA (pi0 or RDT) trained with vision and language only. Feed joint torque as proprioception (p.5).
3. Insert a lightweight VIB module after the pretrained vision-language encoder. Linear layers predict mu and sigma for F_V and F_L. Sample with the reparameterization trick (p.4).
4. Train with L_total = L_task + lambda_VIB(t) * L_VIB. Early: high lambda suppresses vision/language, policy uses torque. Later: lambda decays, vision and language return (p.5).
5. No architectural changes to the VLA backbone; plug-in (p.5).

How it differs (p.2, p.5): FACTR degrades inputs by blur with a heuristic schedule; ForceVLA changes the architecture (MoE); CRAFT is a model-agnostic fine-tuning regularizer. Torque is argued to carry more information than q (p.4).

Key equations:
- VIB loss (p.4, Eq. 3): L_VIB = 1/2 sum_i ( -log sigma_i^2 + mu_i^2 + sigma_i^2 - 1 )
- Reparameterization (p.4, Eq. 4): F^c = mu + sigma * eps, eps ~ N(0, I)
- Impedance torque (p.4, Eq. 5): tau = K(q_d - q) + D(dq_d - dq) + M(ddq_d - ddq) + tau_ext
- Total loss (p.5, Eq. 8): L_total = L_task + lambda_VIB(t) L_VIB
- Schedule (p.5, Eq. 9): lambda_VIB(t) = lambda_init exp(-t / T_decay)
- Values of lambda_init and T_decay: not stated.

## 3. Experiments
- Baselines (p.6): FACTR (force-aware ACT), pi0-base, RDT, pi0-base + CRAFT, RDT + CRAFT.
- Hardware (p.3): Franka Emika Panda follower, low-cost servo leader arm, impedance control, joint torque as force modality, wrist camera + static third-person camera. Tactile sensor: none (CRAFT uses joint torque; tactile feedback is mentioned only as a possible extension, p.5).
- Tasks (Fig. 3, p.5): USB Insertion, Flip Carton, Wipe Whiteboard, Rolling Plasticine, Shaft-to-Hole Insertion. Success criteria on p.5.
- Protocol (p.5-6): 50 teleoperated demos per task. 20 trials for USB, Shaft-to-Hole, Flip Carton; 12 trials for Wipe Whiteboard and Rolling Plasticine.
- Main results (Tab. I, p.6): average success over five tasks: RDT 22.66 -> 48.32 (+25.66); pi0-base 25.32 -> 60.68 (+35.36).
  - Wipe Whiteboard (pi0): 33.3% -> 66.7% (p.6). Rolling Plasticine (RDT): 8.3% -> 58.3% (p.6).
- Generalization (Tab. II, p.7), pi0 w/o -> w/ CRAFT, OOD variants (5 trials per variant; counts are over 10 or 20 as stated): USB Insertion (task-level) 1/10 -> 4/10; Flip Carton (object-level) 10/20 -> 15/20; Wipe Whiteboard (task-level) 2/10 -> 6/10; Rolling Plasticine (object-level) 2/20 -> 12/20; average 22.50% -> 58.75%.
- Ablation (Tab. III, p.7), pi0, 3 tasks (USB, Wipe, Rolling), 10 trials each: base 20.0; + VIB 46.7; + VIB & force (joint torque) 56.7.
- Takeaway: force-first curriculum via VIB gives large gains over the same VLA, across two architectures; VIB alone gives most of the gain, torque adds more.
- Limitations: the paper has no explicit limitations section. Observable from the text: only real-world tests, small trial counts (5-20), only Franka arm, only joint torque (no tactile sensor), no comparison numbers for FACTR in the text (see Fig. 4), lambda hyperparameters not stated, no code/project link. Statements of limitation are my reading, not the paper's.

Slide-worthy figures: Fig. 2 (p.4, method overview, best); Fig. 3 (p.5, five tasks); Fig. 4 (p.6, success bars); Fig. 5 (p.6, qualitative USB/carton); Fig. 6 (p.7, OOD setups); Fig. 1 (p.1, teleop system); Tab. I-III as tables.


---

# Beyond Sight: Finetuning Generalist Robot Policies with Heterogeneous Sensors via Language Grounding (FuSe)

Note: text extracted with PyMuPDF (7 pages; 1-6 main text). Bar-chart values in Figs. 5-8 are not in the text; only Table I values are.

## 0. Paper Information
- Title: Beyond Sight: Finetuning Generalist Robot Policies with Heterogeneous Sensors via Language Grounding (p.1)
- Authors: Joshua Jones*, Oier Mees*, Carmelo Sferrazza*, Kyle Stachowicz, Pieter Abbeel, Sergey Levine (*equal contribution), BAIR, UC Berkeley (p.1)
- Venue/year: not stated in the PDF (arXiv:2501.04693v3 [cs.RO], dated 14 Jan 2025 on p.1)
- Paper URL: https://arxiv.org/abs/2501.04693 (from the arXiv ID on p.1)
- Project URL: https://fuse-model.github.io (p.1)
- Code URL: not stated as a link; the paper says data, code and models are open-sourced (p.2)
- Summary (<=40 words): FuSe finetunes pretrained generalist policies (Octo, a 3B PaliGemma VLA) on touch and audio using two auxiliary losses: CLIP-style contrastive and language generation. Language is the common grounding. It enables zero-shot multimodal prompting and beats vision-only finetuning.

## 1. Motivation
- Generalist policies (Octo, OpenVLA, RT-1) use only vision and proprioception, while humans use touch and audio, e.g. finding keys in a bag (p.1).
- Data scarcity: nearly all robot datasets have vision and proprioception, few have other modalities, and those that do mostly lack robot actions (p.1-2). OXE has no touch or sound (p.2).
- Question posed (p.1): how to keep the generalization of pretrained policies while connecting them to modalities with little data.
- Three finetuning challenges (p.2-3): (i) new modality encoders must be learned from a small dataset; (ii) the model over-relies on pretraining modalities and ignores new sensors with plain MSE finetuning; (iii) cross-modal prompting needs modality-specific annotations.
- Key insight (p.2): ground all modalities in natural language by an auxiliary loss.
- Figure 1 (p.1): the FuSe concept: sensors (wrist and third-person cameras, vision-based tactile, audio spectrogram) fed to Octo or PaliGemma VLA, with contrastive and language generation losses. Example prompts: "Grab the object that feels corded", "Pickup the object that looks yellow and feels crinkled", zero-shot touch description.

## 2. Method
Terminology:
- Heterogeneous sensors: touch (DIGIT vision-based tactile), audio (microphone), plus vision and proprioception (p.4).
- Cross-modal grounding via language: each modality combination is aligned with, or used to generate, a language description.
- Compositional cross-modal prompting: combine modalities in one instruction, e.g. "same color as the button that plays piano" (p.4-5).

Pipeline (Fig. 2, p.3):
1. Tokenize all modalities (wrist image, workspace image, touch, audio, language) and feed to a pretrained transformer backbone (Octo main; PaliGemma 3B VLA also tested) (p.2-3).
2. Tactile encoder: pretrained TVL encoder, finetuned with the backbone; both DIGIT images go through the same encoder (p.3).
3. Audio encoder: 1 s waveform to spectrogram, treated as an image, ResNet26 (p.3-4).
4. Auxiliary loss 1, multimodal contrastive (CLIP-style): observation embedding from all modalities (multi-head attention pooling); one loss per modality-combination instruction, averaged (p.3).
5. Auxiliary loss 2, multimodal generative: a single transformer head with modality tokens predicts the language instruction for each modality combination; cross-entropy (p.3).
6. Language rephrasing: templated after-the-fact annotations (e.g. "feels squishy and is red"), augmented by ChatGPT paraphrases; 20 templates per modality combination (p.3-4).

How it differs: instead of just adding sensor inputs with MSE loss (which ignores them), it adds language-grounding losses; uses small multimodal data on top of a pretrained generalist (p.3).

Key equation (p.3): L = L_BC + beta L_gen + lambda L_contrast, with beta = lambda = 1 (p.4).
Training (p.4): 50,000 steps, v5e-128 TPU pod, batch 1024, cosine LR with 2000 warmup, peak 3e-4.

## 3. Experiments
- Hardware (p.4, Fig. 3): WidowX 250 6-DoF arm, delta EE control at 5 Hz, third-person RGB camera, wrist RGB camera, two DIGIT tactile sensors on the gripper fingers (320x240; static background image subtracted), microphone (1 s of 44.1 kHz audio), 9-DoF IMU.
- Data (p.4): 26,866 teleoperated trajectories (Meta Quest 2 VR), templated language labels. Grasping tasks: vision, touch, action. Button tasks also include sound. Intro says 27K trajectories (p.2).
- Tasks (p.4): Tabletop Grasping, Shopping Bag (occlusion, poor wrist-camera lighting, partially observable), Button Pressing (six sound-making buttons). Compositional tasks: grab object with the same color as the button that plays a given sound; multi-step: press an unseen button, convert the heard sound to an instruction via the generative head, then press the button with the same sound (p.4-5).
- Evaluation: train objects (24 for grasping) and unseen test objects (32); buttons: six training plus two unseen; each scenario with 5 rollouts (p.4).
- Baselines (Fig. 5, p.5): Octo Vision FT (vision-only finetune), Octo from scratch, ResNet from scratch (FiLM language); multi-step baseline: Octo from scratch with the same losses (Fig. 6, p.6). Ablations: no gen loss, no CLIP loss, no aux losses (Fig. 7, p.6).
- Results:
  - Fig. 5 (p.5): FuSe beats scratch baselines. Vision FT is competitive on tabletop and button tasks but clearly worse on the Shopping Bag task. Numeric values not in text (only axis labels).
  - Multimodal prompting, Table I (p.5), vision-ambiguous: average reach/grasp visual-only 0.37/0.34 vs visual+tactile 0.53/0.37. Touch-ambiguous: tactile-only 0.38/0.35 vs visual+tactile 0.45/0.35.
  - Compositional (Fig. 6, p.6): FuSe outperforms baselines, especially on full multi-step completion; scratch model has poor language grounding.
  - Ablation (Fig. 7, p.6): both losses are needed; the baselines degrade, especially on unseen test objects.
  - PaliGemma VLA (Fig. 8, p.6-7): competitive with Octo-FuSe and better on the Shopping Bag task. Authors claim it is the first open-source VLA finetuned on heterogeneous non-visual inputs.
- Abstract claim (p.1): success rate up by over 20% vs all considered baselines.
- Takeaway: language-grounding auxiliary losses make touch and audio useful, most when vision is occluded; the same recipe works for Octo and a VLA.
- Limitations (p.6): extra modalities raise training cost, so observation history is limited to 0.4 s; longer context could help sparse tactile signals. Other (my observations, not the paper's): single robot platform (WidowX), absolute success rates in Table I are modest (0.30-0.55), task-specific data of about 27K trajectories.

Slide-worthy figures: Fig. 1 (p.1, concept and example prompts); Fig. 2 (p.3, architecture); Fig. 3 (p.4, sensor setup); Fig. 4 (p.4, objects); Fig. 5 (p.5, main results); Table I (p.5); Fig. 6 (p.6, compositional); Fig. 7 (p.6, ablation); Fig. 8 (p.7, VLA).


---

# Towards Human-Like Manipulation through RL-Augmented Teleoperation and Mixture-of-Dexterous-Experts VLA (MoDE-VLA)

Note: text extracted with PyMuPDF (8 pages); figure contents inferred from captions and text only.

## 0. Paper Information
- Title: Towards Human-Like Manipulation through RL-Augmented Teleoperation and Mixture-of-Dexterous-Experts VLA (p.1)
- Authors: Tutian Tang*, Xingyu Ji*, Wanli Xing*, Ce Hao, Wenqiang Xu, Lin Shao, Cewu Lu, Qiaojun Yu+, Jiangmiao Pang+, Kaifeng Zhang+ (*equal, +corresponding) (p.1)
- Affiliations (p.1): Shanghai Jiao Tong University, Sharpa, Shanghai AI Lab, Zhongguancun Academy, National University of Singapore, Shanghai Innovation Institute.
- Venue/year: not stated (arXiv preprint). arXiv:2603.08122v1 [cs.RO], 9 Mar 2026 (p.1)
- Paper URL: https://arxiv.org/abs/2603.08122 (from the arXiv ID on p.1)
- Project URL: not stated (a supplementary video is mentioned, p.3)
- Code URL: not stated
- Summary (<=40 words): IMCopilot (RL in-hand skills) assists teleoperation and is callable by the VLA. MoDE-VLA injects force and tactile tokens into pi0 via self-attention, sparse MoE, and residual injection. Average success on four dexterous tasks: 34% vs 15% for pi0.

## 1. Motivation
- VLAs are mostly limited to low-DoF grippers and vision-guided pick-and-place; the paradigm breaks for in-hand manipulation (p.1).
- Three challenges for bimanual dexterous manipulation (p.1-2):
  - Data acquisition: teleoperating 63 DoF is hard; in-hand coordination often exceeds even expert operators.
  - Multi-skill learning: apple peeling mixes vision-guided gross motion, force-guided cutting, tactile-guided in-hand rotation, hard for one policy.
  - Modality heterogeneity: directly concatenating force/tactile into a pretrained VLA often degrades performance (cites ForceVLA etc.).
- Prior work (p.2): vision/glove/exoskeleton teleop lacks force feedback or leaves in-hand parts to the human; RDP, TA-VLA, ForceVLA, Tactile-VLA are mostly limited to two-finger grippers; high-DoF dexterous-hand integration is "largely unexplored".
- Figure 1 (p.1): (a) RL-augmented teleop with IMCopilot and force/tactile feedback, (b) apple peeling as the ultimate task, (c) other tasks: tube rearranging, charger plugging, gear assembling.

## 2. Method
Terminology:
- IMCopilot: In-hand Manipulation Copilot; RL-trained atomic skills: stable grasp maintenance and in-hand rotation about a specified axis (p.3).
- Shared autonomy: operator controls arms via exoskeleton and triggers hand skills with foot pedals (p.3).
- MoDE: Mixture-of-Dexterous-Experts module (self-attention + sparse token-level MoE + residual injection) (p.4-5).
- Force = joint torque of both arms (14-d); tactile = 6-DoF force/wrench from ten fingertip sensors (60-d) (p.5).

Pipeline:
1. Data collection (Fig. 2, p.3): exoskeleton + exoskeleton gloves + VR headset with force/tactile overlays and vibrotactile fingertip feedback; IMCopilot triggered by foot pedal for in-hand phases.
2. IMCopilot training (p.4): PPO in IsaacLab, asymmetric actor-critic with teacher-student distillation, privileged info for the teacher, domain randomization for zero-shot sim-to-real. Policy outputs relative joint offsets. Reward: rotation velocity about the axis minus penalties (linear velocity, work, torque, joint deviation).
3. VLA backbone (p.4-5): pi0 (SigLIP, PaliGemma Gemma-3B, Gemma-300M action expert), flow matching.
4. Force/tactile tokens (p.5): linear projection to PaliGemma width; replicate H times with sinusoidal step encodings so the router can assign different experts per timestep.
5. MoDE block (p.5): concatenate [prefix, suffix, force tokens, tactile tokens], run self-attention, then top-k sparse MoE (E = 8, k = 1).
6. Residual injection (p.5): modality-specific heads: force tokens refine arm actions, tactile tokens refine hand actions, added to the suffix output.
7. Hierarchical decision (Fig. 3, p.4): the VLA outputs a scalar trigger c in [0,1]; c > 0.5 hands the hand actions to IMCopilot (in-hand rotation) while arm control stays with the VLA (p.4-5).

How it differs (p.4-5): dedicated force/tactile pathway rather than appending them to the state vector; modality-aware routing; residual structure so correction goes to about zero in free space, preserving pretrained knowledge; skill-level shared autonomy for data collection plus reuse as a low-level skill.

Key equations:
- Flow matching loss (p.5, Eq. 1): L_FM = E || v_theta(x_t, t) - (eps - x_0) ||^2, with x_t = t eps + (1-t) x_0, t ~ Beta(1.5, 1)
- Euler inference (p.5, Eq. 2): x_{t+dt} = x_t + dt v_theta(x_t, t), dt = -1/N
- Token replication (p.5, Eq. 3): z~_f^(h) = z_f + PE_sin(h), same for tactile, h = 1..H
- MoDE input (p.5, Eq. 4): Z_in = [Z_prefix || Z_suffix || Z~_f || Z~_g]
- Residual injection (p.5, Eq. 5): v_theta(x_t, t) = [ W1(Z_f + Z_suffix) || W2(Z_g + Z_suffix) ]

## 3. Experiments
- Hardware (p.3): SharpaNorth dual 7-DoF arms with 22-DoF SharpaWave hands; each fingertip has a vision-based tactile sensor giving 6-DoF force/wrench (via the hand API); 2-DoF neck, 3-DoF upper body; 63 active DoF. Cameras: two head RGB (stereo), two wrist fish-eye. Joint torque from both arms.
- Training details (p.5-6): init from pi0 checkpoint, MoDE from scratch; E = 8, top-k = 1, H = 50, N = 10 Euler steps; three views at 224x224; 60,000 steps, AdamW, cosine decay.
- Tasks (Fig. 4, p.6): Apple Peeling (dual arm; IMCopilot drives the left hand), Tube Rearranging (dual arm), Gear Assembling (single arm, 3 gears), Charger Plugging (single arm). Other tasks use the VLA for the hands directly.
- Protocol (p.6): 20 trials per task; Success Rate (SR) and Peel Completion Ratio (PCR, 25% increments, apple only). Baseline: pi0 only.
- Q1, data collection (p.6-7): force/tactile feedback in Gear Assembling: without 85 successes of 100 trials in 75 min; with 93 of 100 in 65 min. In-hand skills (Table I, p.6): teleoperation 31/90 (34%) vs IMCopilot 80/90 (89%); ping-pong ball 10% vs 83%, tennis ball 67% vs 93%, apple 27% vs 90%.
- Q2, policy (Table II, p.7), SR: pi0 vs MoDE-VLA: Apple Peeling 0% (failure) vs 30% (PCR 8% vs 73%); Tube Rearr. 15% vs 30%; Gear Assem. 40% vs 60%; Charger Plug. 5% vs 15%; Average 15% vs 34% (+19).
- Q3, ablations (Table II, p.7): w/o Force: Apple 15% (PCR 48%), Tube 25%, Gear 45%, Charger 5%, Avg 23%. w/o Tactile: Apple 30% (PCR 70%), Tube 15%, Gear 45%, Charger 15%, Avg 26%. w/o IMCopilot: Apple PCR 25% (VLA learns hand actions from IMCopilot demos directly; the SR cell is "/").
- Takeaway: force removal hurts most (-11 avg SR); tactile -8; IMCopilot is essential for apple peeling. The abstract states "doubled success rate improvement over the baseline".
- Limitations: there is no explicit limitations section. Observable from the text (my reading): absolute success rates are low (15-60%, 20 trials per task); only pi0 as baseline (no ForceVLA or other force-aware VLA comparison); one proprietary robot platform; IMCopilot covers only grasp-maintenance and rotation skills and was used only in apple peeling; tactile ablation does not hurt apple peeling since IMCopilot already uses tactile input (p.7).

Slide-worthy figures: Fig. 1 (p.1, overview + apple peeling); Fig. 3 (p.4, architecture, best for Method); Fig. 2 (p.3, teleop and robot platform); Fig. 4 (p.6, task key frames); Table I (p.6); Table II (p.7).


---

# TLA: Tactile-Language-Action Model for Contact-Rich Manipulation

Note: text was extracted with PyMuPDF (all 7 pages read). Figure contents are inferred from captions and extracted text only.

## 0. Paper Information
- Title: TLA: Tactile-Language-Action Model for Contact-Rich Manipulation (p.1)
- Authors: Peng Hao*, Chaofan Zhang*, Dingzhe Li, Xiaoge Cao, Xiaoshuai Hao, Shaowei Cui (corresponding), Shuo Wang (* equal contribution) (p.1)
- Affiliations (p.1): Samsung Research China - Beijing; State Key Lab of Multimodal AI Systems, Institute of Automation, CAS; Beijing Academy of Artificial Intelligence.
- Venue/year: not stated in the PDF (arXiv preprint, arXiv:2503.08548v1 [cs.RO], 11 Mar 2025, p.1)
- Paper URL: https://arxiv.org/abs/2503.08548 (from arXiv ID, p.1)
- Project URL: https://sites.google.com/view/tactile-language-action/ (p.1)
- Code URL: not stated as a separate link; the paper says data, models and code are on the project website (p.7)
- Summary (<=40 words): TLA fine-tunes Qwen2-VL 7B to map a tiled sequence of fingertip tactile images plus a text instruction to peg-in-hole actions. Trained on 24k simulated samples, it beats BC and Diffusion Policy and gets over 85% success on unseen clearances and peg shapes.

## 1. Motivation
- Tactile sensing is crucial for contact-rich tasks such as fine assembly (p.1).
- Existing tactile methods rely on specialized models trained on specific datasets and generalize poorly (p.1).
- Most VLA models are vision-focused and lack tactile input, which limits contact-rich use (p.1).
- Existing tactile-language work is perception-only, lacks robot actions, or treats tactile as a supplementary modality; confined to perception or pick-and-place grasping (p.1).
- Two stated challenges (p.1): (1) no tactile-action instruction dataset for contact-rich manipulation; (2) no suitable tactile-language-action model.
- Claim: first language-grounded tactile-action generation framework, and first language-action model based solely on tactile perception (p.2).
- Fig. 1 (p.1): overview. Tactile data collection builds the "TLA Data"; the TLA model is a Tactile Encoder + Qwen Decoder (Qwen2-VL); prompt "Please insert the square peg into the target hole." and answer "Based on the tactile observations, the action is [dx, dy, drz]". Caption says it is a 7B multimodal LM fine-tuned (SFT) to control the robot on peg-in-hole.

## 2. Method
Terminology:
- Visuotactile sensor: GelStereo 2.0 outputs tactile images (p.2). Left and right fingertip sensors are used.
- Peg-in-hole action (dx, dy, drz): planar translation and rotation about z that adjusts the peg pose after a failed insertion (p.2).
- Clearance c: assembly clearance between peg and hole (p.2).
- GCR (Goal Convergence Rate): percentage of output actions that are correct in all of x, y, rz (p.4).

Pipeline (Fig. 2, p.3):
1. Data collection in NVIDIA Isaac Gym (p.2): FEM-based tactile deformation via Flex physics engine; tactile imprint rendering from [28] with a real-sensor texture to narrow the sim-real gap. The gripper grasps a peg, starts with random 3-DoF misalignment (x, y, rz), descends; on collision the attempt fails and the tactile sequence is recorded. Max 15 attempts. A random insertion policy is used in simulation; labels come from the peg-hole pose error (Eqs. 1-3, p.2).
2. Instruction format (Fig. 3, p.3): Qwen chat template with vision start/end tokens around the tactile image; the answer is an action string such as "[-0.9, 0.4, 0.013]".
3. Tactile encoder (p.3): 4 timestamps x left/right = 8 tactile images arranged in a 3x3 grid (last cell white), resized to 616x616. The Qwen2-VL ViT encodes it (temporal information converted to spatial layout); an MLP compresses each 2x2 patch group to one token, giving 1936 tactile tokens (patch size 14).
4. Action prediction (p.3-4): Qwen2 7B is the backbone. Numerical text encoding of the pretrained tokenizer is kept (not replaced by OpenVLA-style special bin tokens). Because Qwen2 tokenizes digits individually, ground-truth actions are scaled by a factor s and rounded to integers (A_gt = A_raw * s).
5. Training (p.4): tactile encoder frozen; Qwen2 7B fine-tuned with LoRA; next-token-prediction loss. Inference uses beam search until end token, then an Action-De-Tokenizer converts text to floats.

How it differs from existing methods:
- vs. VLA (OpenVLA, RT-2): tactile-only input, no vision; keeps native numeric tokens rather than overwriting rare tokens as bins (p.3).
- vs. FuSe (Jones et al.): TLA connects tactile perception to action using a pretrained language model, tactile-only (p.2).
- vs. BC / Diffusion Policy: language-model backbone fine-tuned rather than a policy trained from scratch (p.4).

Key equations:
- Label generation (p.2, Eqs. 1-3): dx_hat = Fclip(-ex + c/2, -delta, 0) if ex >= 0, else Fclip(-ex - c/2, 0, delta) (same form for y); drz_hat = Fclip(-erz, -1.5 deg, 1.5 deg); delta = 1 mm.
- NTP loss (p.4, Eq. 4): L_NTP = -sum_t p(y_t) log P(y_t | y_<t, x).
- L1 metric (p.4, Eq. 5): L1 = (1/m) sum |y_i - y_hat_i|.
- Pseudocode (my paraphrase, not in the paper):
```
obs = tiled 3x3 grid of 8 tactile images (L/R x t=0..3)
tokens = ViT(obs) -> MLP(2x2 merge) + text_instruction
action_text = Qwen2-7B(tokens)        # LoRA fine-tuned, NTP loss
[dx, dy, drz] = detokenize(action_text) / s
```

## 3. Experiments
Baselines (p.4): Behavior Cloning (ResNet-50, tactile image input), Diffusion Policy (DP), plus TLA variants SP-TLA (square pegs only) and MP-TLA (square + triangular pegs).

Dataset (p.1-2, p.4): 24k tactile sequences with action trajectories, all with 2.0 mm clearance; simulated. Training on 8x NVIDIA A6000 (20 epochs single-peg, 10 epochs multi-peg).

Tasks:
- Single-peg offline (Table I, p.4): 8k square-peg samples, 6k train / 2k test.
- Multi-peg offline (Table II, p.5): train 16k (square + triangle, equal split); test 8k (4k ID square/triangle, 4k OOD round/hexagon).
- Closed-loop insertion (Tables III-IV, p.5-6): square peg at clearances 2.0/1.6/1.0 mm; square/triangle/round/hexagon pegs at 2.0 mm. Each task repeated 50 times; max 15 attempts. Metrics: success rate and average steps of successful episodes (p.6).

Hardware / sensors:
- GelStereo 2.0 visuotactile sensors on the gripper fingertips (p.2). The robot arm model is not stated. Closed-loop insertion appears to be in simulation (the Discussion says real-world deployment is future work, p.7); the paper does not say explicitly which environment Tables III-IV use, so treat as "not stated, likely simulation".

Key numbers:
- Table I (p.4), single peg: GCR BC 10.4 / DP 8.5 / SP-TLA 12.5. L1 x (mm): 0.803 / 0.370 / 0.079; L1 y: 0.302 / 0.382 / 0.122; L1 rz (deg): 0.205 / 0.568 / 0.173. x-direction L1 is 78% lower than second best (p.4).
- Table II (p.5), multi-peg ID GCR: BC 18.4, DP 7.4, MP-TLA 18.4; MP-TLA L1 x/y/rz ID = 0.102/0.114/0.135. OOD GCR: BC 0.152 (as printed), DP 0.080, MP-TLA 0.165 (extracted table; column alignment of OOD GCR may be ambiguous).
- Table III (p.5), clearance success %: 2.0 mm: BC 44, DP 58, SP-TLA 96, MP-TLA 94. 1.6 mm: 32 / 43 / 94 / 86. 1.0 mm: 18 / 20 / 74 / 90. Total: 31 / 40 / 88 / 90. Text: surpasses second best by 50% in success rate (p.6); abstract: over 85% success at 1.6 and 1.0 mm when trained only on 2.0 mm.
- Table IV (p.6), peg-type success %: Square (ID) BC 60, DP 60, SP-TLA 96, MP-TLA 94; Triangle (ID) 84 / 60 / 76 / 92; Round (OOD) 62 / 56 / 92 / 94; Hexagon (OOD) 66 / 54 / 90 / 96. MP-TLA beats SP-TLA on OOD.
- Steps: TLA is not always fewer steps (e.g., 2.0 mm: BC 2.60, DP 2.45, SP-TLA 3.15 steps) (Table III).

Takeaway: a language-model backbone fine-tuned on tiled tactile images gives much more accurate actions and far higher insertion success than BC/DP, and generalizes to unseen clearances and shapes; training on more peg types improves OOD (p.6).

Limitations (p.5-7):
- 2D tactile images represent the direction perpendicular to the gripper (y) poorly, causing more y errors; this explains the triangular peg failure case (Fig. 6, p.6; allowable deviation 3d along x vs 2.3d along y).
- Temporal information is encoded as spatial layout, not true sequence modeling (p.7).
- Tactile signal format is rudimentary (2D images only; depth maps, point clouds not tried); action de-tokenization is simplistic (p.7).
- Sim-to-real not yet evaluated (future work, p.7).
- Low absolute GCR (max 18.4%), though the paper reports it as relative to baselines.

Figures worth screenshotting:
- Fig. 1 (p.1): system overview.
- Fig. 2 (p.3): architecture (tactile encoder + Qwen2 7B + LoRA + de-tokenizer).
- Fig. 3 (p.3) dataset example / prompt format (small).
- Fig. 5 (p.6): snapshots of hexagonal peg insertion after two attempts.
- Fig. 6 (p.6): failure case analysis.
- Tables III and IV (p.5-6): main results.


---

# VLA-Touch: Enhancing Vision-Language-Action Models with Dual-Level Tactile Feedback

Note: text was extracted with PyMuPDF; all 18 pages (main text, references, appendix A-C) were read. Figure contents are inferred from captions and extracted text only.

## 0. Paper Information
- Title: VLA-Touch: Enhancing Vision-Language-Action Models with Dual-Level Tactile Feedback (p.1)
- Authors: Jianxin Bi, Kevin Yuchen Ma, Ce Hao, Mike Zheng Shou, Harold Soh (Bi and Soh: correspondence) (p.1)
- Affiliations: Dept. of Computer Science, National University of Singapore; Smart Systems Institute, NUS (p.1)
- Venue/year: not stated on the page text read (arXiv preprint, arXiv:2507.17294v2 [cs.RO], 29 Jul 2025, p.1)
- Paper URL: https://arxiv.org/abs/2507.17294 (from the PDF hyperlink, p.1)
- Project URL: not stated
- Code URL: https://github.com/jxbi1010/VLA-Touch (hyperlink behind "Code is open-sourced at this URL", p.1)
- Summary (<=40 words): VLA-Touch adds tactile feedback to a frozen VLA at two levels: Octopi tactile-language descriptions feed a GPT-4o planner, and a tactile-conditioned interpolant diffusion controller refines RDT action chunks. Real-robot cup, wipe and peel tasks improve over vision-only baselines.

## 1. Motivation
- Vision cannot reliably determine compliance, texture or contact events; VLAs lack tactile input and there are no large multimodal tactile datasets (p.1).
- Two challenges (p.1): (1) how to acquire and plan with tactile information when pretrained models never saw tactile data; (2) how to integrate touch into VLA pipelines whose base models have no tactile modality.
- Hypothesis: tactile helps at two levels. Planning level: object properties such as hardness and roughness that are visually ambiguous. Control level: contact dynamics such as friction and contact events (p.1).
- Existing work (p.3): other multi-sensor VLA extensions focus on task-level reasoning, not precise manipulation; tactile policies address control for specific tasks; [27] (the TLA-style work) is tactile-language policy but excludes vision.
- Design philosophy: modular and loosely coupled, no fine-tuning of the base VLA with tactile data (p.1-2).
- Fig. 1 (p.2): the dual-level framework. Planning: user instruction and scene image go to a VLM Task Planner, which outputs instruction I_k to the VLA; Octopi converts a tactile sequence to a language description fed back to the planner. Control: the VLA produces an action chunk, then an interpolant model refines it using DinoV2 visual embeddings and low-dimensional tactile signals from marker tracking.
- Inspired by the human neural system: planner ~ prefrontal cortex, tactile-language model ~ secondary somatosensory cortex, tactile controller ~ posterior parietal cortex (p.2).

## 2. Method
Terminology:
- Octopi: pretrained tactile-language model for GelSight, infers object properties (roughness, hardness) from a 6-frame tactile sequence (p.4, ref [35]).
- RDT (Robot Diffusion Transformer, RDT-1B): base VLA (p.3, p.6).
- BRIDGeR / interpolant policy: a diffusion-like controller using stochastic interpolants that starts from an informative source distribution (here the VLA's action chunk) instead of Gaussian noise (p.4, p.17).
- Marker tracking: GelSight Mini has a 7x9 marker array; per-marker force vectors are summed into m_t = (X, Y, M) = aggregated force vector and magnitude (p.4).

Pipeline (Algorithm 1, p.5):
1. GPT-4o planner gets goal g and scene image, issues one primitive instruction I_k (one action, at most one object) (p.4).
2. RDT generates an action chunk (T_a = 64) from state s_t and I_k (p.17).
3. The interpolant controller refines the chunk in a sliding window: the first 48 of 64 steps are used; non-overlapping segments are refined and executed sequentially at up to 8 Hz (p.4, p.18).
4. When the instruction finishes or max steps is exceeded, Octopi converts recent tactile frames to text (properties); for force, the marker-tracking force vector plus reference values is given to GPT-4o instead (Octopi has no force pretraining) (p.7).
5. GPT-4o updates the next instruction I_{k+1} from (g, o_t, L_t, I_k) (p.5).

Training of the controller (p.17-18): supervised, paired (VLA-generated chunk, expert chunk) from kinesthetic demonstrations; observation z_t = [proprioception, DinoV2 embeddings, tactile force]; MSE to expert actions. RDT was first fine-tuned on unrelated Franka data (100k steps, ~80 h), then per task (20k steps, ~16 h), on one RTX 4090.

How it differs from existing methods:
- vs. TLA/VTLA-style models: base VLA is not fine-tuned with tactile; tactile enters only through a language planner and a residual-like refiner (p.1-2).
- vs. a simple residual controller: interpolant diffusion models multi-modal action distributions better (p.8).
- vs. raw tactile images to a VLM: linguistic descriptions are more usable by GPT-4o (p.7).

Key equations (appendix C, p.17-18):
- Interpolant: a_t = I(t, a_0, a_1, x) + gamma(t) z, with I(0)=a_0 (VLA action), I(1)=a_1 (expert action), gamma(0)=gamma(1)=0.
- Forward SDE: da_t = b_F(t, a_t, x) dt + sqrt(2 eps(t)) dW_t.
- Training loss: mean over D_VLA of || a_exp - pi_I(a_hat | a_VLA, z_t; theta) ||^2.

## 3. Experiments
Hardware (p.5): Franka Emika Panda arm, Robotiq 2F-140 gripper, one GelSight Mini tactile sensor on one finger (7x9 marker gel), two RealSense cameras (overhead scene camera, wrist camera), RTX 4090 workstation.

Tasks (p.6): Cup (lidded cup; use touch to tell if it holds water; place at corresponding location without spilling), Wipe (choose the smoother of two sponges, wipe ink off a plate), Peel (choose the softer/harder mango, peel it with a hand-held peeler). Demonstration data via kinesthetic teaching: Cup 100 episodes (~8 min), Wipe 100 (~15 min), Peel 180 (~30 min) at 10 Hz (p.17).

Baselines (p.5-6):
- Planning: GPT-4o (scene image only), GPT-4o + raw tactile images, GPT-4o + Octopi (ours).
- Control: RDT, RDT + Residual controller (RNN), RDT + Interpolant controller (ours); ablations w/o Touch and w/o Vision.
- Dual-level: w/o Planning (tactile only in control), w/o Control (tactile only in planning), full VLA-Touch.

Key numbers:
- Planning (Fig. 3C, p.7; 20 trials each): GPT-4o scene-only is non-committal / chance-level. Raw tactile images: roughness 100%, force 50%, hardness 60%. Octopi/linguistic: force 90%, hardness 75% (roughness not given in the text). Abstract: planning efficiency up to 40% better (p.2).
- Manipulation (Table 1, p.7; success out of 20): Cup pick/place: RDT 9/7, Residual 7/6, Interpolant 12/10, w/o Touch 10/5, w/o Vision 10/7. Wipe full: RDT 5, Residual 6, Interpolant 12, w/o Touch 7, w/o Vision 8. Peel full: RDT 6, Residual 7, Interpolant 10, w/o Touch 5, w/o Vision 5. Interpolant improves base RDT task success by 42% (Cup), 140% (Wipe), 67% (Peel) (p.7).
- Dual-level (Table 2, p.8; interleaved plan + manipulation, out of 20): Cup w/o Planning 5, w/o Control 6, VLA-Touch 9; Wipe 5 / 5 / 12; Peel 6 / 4 / 7. Up to 35% higher task success than single-level variants (p.2).
- Appendix B (p.16-17): Octopi's discretized language descriptions lose granularity. Wipe: 100% either way; Mango (subtle hardness difference): full Octopi language 60% vs continuous classifier output 75%.

Takeaway: tactile feedback helps both planning (language descriptions beat raw images for GPT-4o) and control (interpolant beats RDT and residual controllers); both levels are needed for best task success; vision and touch are both needed in the controller (p.8-9).

Limitations (p.9):
- Gripper/control setup differs from the Octopi pretraining data, hurting contact interpretation, especially hardness.
- Generalization tested mostly across object positions and target locations, not across tasks; each task needs its own training.
- Controller runs at 8 Hz and does not exploit high-frequency tactile signals (>=25 Hz).
- Absolute success rates remain modest (e.g., 7/20 on Peel full system, 9/20 on Cup); only 20 trials per setting, no variance reported (observation from tables, not stated by the paper).
- Language is a lossy representation of tactile properties (Appendix B).

Figures worth screenshotting:
- Fig. 1 (p.2): dual-level framework diagram (most informative).
- Algorithm 1 (p.5): pseudocode.
- Fig. 2 (p.6): three-task pipeline (Cup, Wipe, Peel) with planning and manipulation stages.
- Fig. 3 (p.7): raw tactile vs linguistic tactile feedback and planning success bars.
- Fig. 4 (p.8): peeling result comparison (RDT vs Residual vs Interpolant).
- Table 1 (p.7) and Table 2 (p.8).


---

# VTLA: Vision-Tactile-Language-Action Model with Preference Learning for Insertion Manipulation

Note: text was extracted with PyMuPDF; all 15 pages (main text, references, appendix A-C) were read. Figure contents are inferred from captions and extracted text only.

## 0. Paper Information
- Title: VTLA: Vision-Tactile-Language-Action Model with Preference Learning for Insertion Manipulation (p.1)
- Authors: Chaofan Zhang*, Peng Hao*, Xiaoge Cao, Xiaoshuai Hao, Shaowei Cui (corresponding), Shuo Wang (* equal contribution) (p.1)
- Affiliations: State Key Lab of Multimodal AI Systems, Institute of Automation, CAS; Samsung R&D Institute China-Beijing; Beijing Academy of Artificial Intelligence (p.1)
- Venue/year: not stated (arXiv preprint, arXiv:2505.09577v1 [cs.RO], 14 May 2025, p.1)
- Paper URL: https://arxiv.org/abs/2505.09577 (from arXiv ID, p.1)
- Project URL: the abstract says "please visit our project website: VTLA" (p.1), but no URL is extractable from the PDF; not stated.
- Code URL: not stated
- Summary (<=40 words): VTLA fine-tunes Qwen2-VL 7B on wrist-camera and fingertip tactile images using vision-guided temporally enhanced tokens, then applies DPO for regression-like supervision. Trained only in simulation, it beats DP, VLA and TLA and reaches 95% real-world peg insertion.

## 1. Motivation
- Contact-rich insertion needs vision for global perception and touch to resolve uncertainty (p.1-2).
- Existing vision-tactile methods use proprietary models trained on specific datasets; limited generalization (p.2).
- VLAs generalize well but mostly lack tactile feedback, limited to simple pick-and-place (p.2).
- TLA (prior work by the same group) has two limitations (p.2): (1) its tactile encoding and training/inference framework have room for improvement; (2) no vision, which sets a performance ceiling because the robot lacks global perception.
- NTP (classification) loss mismatches continuous robotic regression, limiting the SFT model (p.5).
- Fig. 1 (p.1): VTLA overview; policy learned from domain-randomized simulated insertion data (vision, tactile, language inputs) and deployed to real-world multi-peg insertion; example prompt "Please insert the triangle peg into the hole." and answer "The action is [dx, dy, drz]".

## 2. Method
Terminology:
- VGTE (Vision-Guided Temporally Enhanced tokens): input design for the VLM (p.4).
- DPO (Direct Preference Optimization): preference-learning objective used here to emulate regression supervision (p.5).
- GCR (Goal Convergence Rate): percentage of actions correct in all of x, y, rz (p.6).

Pipeline (Fig. 3, p.5):
1. Data (p.3-4, Fig. 2): Isaac Gym peg-in-hole with a self-built visuotactile simulator "TacFlex"; wrist camera + fingertip tactile sensors. 3-DoF random misalignment (x, y, rz), descend, collision = failure, retract, max 15 attempts. 28,000 samples over 5 peg shapes (round, square, pentagon, hexagon, triangle), clearances 0.6-2.0 mm. Each sample: left/right tactile sequences, one visual image, action label. Tactile sequences arranged in a 2x2 grid. Domain randomization on physics (Young's modulus, Poisson ratio, friction), task parameters, tactile color jitter, lighting, image scale/translate/rotate/shear, noise and blur (Appendix A, Table 8, p.13-14).
2. VGTE input (p.4): (a) vision guidance: visual tokens are placed after tactile tokens (closer to the action prediction) to counter LLM recency bias; (b) temporal enhancement: tactile sequences are encoded into image-like grids and a ViT extracts temporally-aware tactile tokens.
3. Stage 1 SFT (p.4-5): next-token prediction on action text, vision encoder and adapter frozen, only LLM tuned; Qwen2-VL 7B; LLaMA-Factory; lr 5e-4, batch 64, 10 epochs (p.6).
4. Stage 2 DPO (p.5-6): sample diverse actions from the SFT model on training inputs; label those closer to ground truth as chosen and others as rejected; optimize DPO with reference model = SFT model. lr 5e-6, batch 32, 3 epochs; 1k and 2.4k preference pairs (p.6).

How it differs from existing methods:
- vs. TLA: adds wrist-camera vision, VGTE token ordering and tactile temporal encoding, and DPO stage (p.2).
- vs. VLA (OpenVLA-style): adds tactile input; DPO regression-like supervision (p.2, p.5).
- vs. DP: language-model backbone instead of a from-scratch policy (p.6).

Key equations:
- NTP loss (p.5, Eq. 1): L_NTP = -sum_n log P_theta(x_n | x_<n).
- DPO loss (p.5, Eq. 2): L_DPO = -log sigma( beta log [pi_theta(y_chosen|x) / pi_ref(y_chosen|x)] - beta log [pi_theta(y_rejected|x) / pi_ref(y_rejected|x)] ).
- Pseudocode (my paraphrase, not in the paper):
```
Stage1: SFT on (tactile grid L/R, wrist image, prompt) -> "[dx, dy, drz]"   # NTP
Stage2: for x in train: sample actions; chosen = closer to GT, rejected = farther
        minimize DPO loss with ref = SFT model
```

## 3. Experiments
Baselines (p.6): Diffusion Policy (vision + tactile observations), VLA (wrist image only, Qwen2-VL), TLA (tactile sequences only), VTLA ablations: w/o DPO, w/ DPO-1k, w/ DPO-2k.

Tasks and benchmarks:
- Offline dataset (Table 1, p.6): 6k ID and 4k OOD samples.
- Simulated insertion (Tables 2-3, p.6-7): square peg with clearances 2.0/1.6/1.0/0.6 mm; five peg shapes at 0.6 mm (square, triangle ID; hexagon, pentagon, round OOD per Table 3 header). 50 trials each.
- Real-world (Tables 5-7, p.7-8): 20 trials per setting; sim-to-real zero-shot; squares at 1.6/1.0/0.6 mm; five pegs at 0.6 mm; comparisons VLA/TLA/VTLA on triangle and pentagon at 0.6 mm. Misalignment sampled in -2.5 to 2.5 mm (x, y) and -5 to 5 deg (rz) (p.14).
- Appendix C (p.14-15, Figs. 6-7): dim lighting comparison, VTLA vs VLA.

Hardware (p.6, p.14): 6-DoF UR3 arm, Robotiq 2F-85 gripper, Intel RealSense D405 wrist camera, two GelStereo 2.0 visuotactile sensors on the fingertips (tactile 4-frame sequences per sensor, ROS at 20 FPS).

Key numbers:
- Table 1 (p.6) GCR ID/OOD: DP 7.8/8.5, VLA 46.1/29.5, TLA 15.3/14.4, VTLA 47.3/31.2.
- Table 2 (p.6) sim success % at 2.0/1.6/1.0/0.6 mm: DP 42/32/28/22; VLA 100/98/90/80; TLA 94/90/80/80; VTLA 100/98/96/90.
- Table 3 (p.7) sim 0.6 mm by shape (success %): Square VTLA 90 (VLA 80, TLA 80, DP 22); Triangle 88 (82, 74, 30); Hexagon 90 (84, 80, 28); Pentagon 92 (82, 80, 26); Round 92 (94, 92, 10). Round is similar across LLM models (attributed to isotropy).
- Table 4 (p.7) DPO ablation: OOD GCR w/o DPO 27.0, DPO-1k 31.4, DPO-2k 31.2; ID GCR about 47.5 for all. Text: 16% GCR improvement and ~10% L1 reduction on OOD; more preference data did not help.
- Real world (Tables 5-6, p.7): square 1.6/1.0/0.6 mm success 100/100/95 %, steps 1.60/1.95/4.31; 0.6 mm shapes: square 95, triangle 95, hexagon 95, pentagon 100, round 100 (% success).
- Real world comparison (Table 7, p.8), 0.6 mm: Triangle: VLA 90 %, TLA 30 %, VTLA 95 %; Pentagon: VLA 100, TLA 40, VTLA 100. VTLA 1.85 steps vs VLA 2.3 on pentagon.
- Fig. 4 (p.8): Case 2, VTLA completes in 3 steps while TLA fails after 15 attempts. Appendix C: under dim light VTLA succeeds (3 steps) while VLA fails after 15 steps (Figs. 6-7, p.15).

Takeaway: vision is the dominant signal (VLA and VTLA far above TLA), tactile adds gains in precision and sample efficiency (steps) and robustness under poor lighting, and DPO helps OOD generalization; sim-only training transfers to real robot (p.7-8).

Limitations (p.9):
- Tactile is encoded by an off-the-shelf vision encoder, possibly losing tactile-specific features; dedicated tactile-language alignment is needed.
- Deep vision-tactile feature fusion for LLMs is unexplored.
- Observations from tables (not stated by the paper): gains of VTLA over VLA are small in simulation (e.g., GCR 47.3 vs 46.1; 100 vs 100 at 2.0 mm), and ID GCR is under 50%; real-world trials are only 20 per setting; DPO preference data came from only two sampling configurations (p.7).
- Authors suggest more diverse preference data may matter more than more data (p.7).

Figures worth screenshotting:
- Fig. 1 (p.1): overview.
- Fig. 2 (p.3): data collection and examples (five pegs, tactile sequence noise variants).
- Fig. 3 (p.5): two-stage pipeline (VGTE + SFT, then DPO).
- Fig. 4 (p.8): real-world snapshots VLA vs TLA vs VTLA.
- Tables 2, 3 (p.6-7) and Tables 5-7 (p.7-8): main results.
- Fig. 5 (p.14): real robot setup. Figs. 6-7 (p.15): dim light comparison.


---

# Tactile-VLA: Unlocking Vision-Language-Action Model's Physical Knowledge for Tactile Generalization

Note: text extracted with PyMuPDF (Read tool could not render pages); all 13 pages (10 body + references) were read as text. Figure contents are inferred from text/captions only.

## 0. Paper Information
- Title: Tactile-VLA: Unlocking Vision-Language-Action Model's Physical Knowledge for Tactile Generalization (p.1)
- Authors: Jialei Huang*, Shuo Wang*, Fanqi Lin, Yihang Hu, Chuan Wen, Yang Gao (*equal contribution) (p.1)
- Affiliations (p.1): Tsinghua University; UESTC; Shanghai Jiao Tong University.
- Venue/year: arXiv preprint, arXiv:2507.09160v1 [cs.RO], 12 Jul 2025 (p.1). Peer-reviewed venue: not stated.
- Paper URL: https://arxiv.org/abs/2507.09160 (derived from the arXiv ID on p.1)
- Project URL: not stated in the PDF.
- Code URL: not stated in the PDF.
- Summary (<=40 words): Tactile-VLA adds tactile input and force targets to a pi0-based VLA, with a hybrid position-force controller and a chain-of-thought variant. With few demos it grounds words like "gently" in force and generalizes zero-shot to new tasks and objects.

## 1. Motivation
- Current VLAs excel at high-level reasoning but are weak at grounding decisions in fine-grained physical reality, especially force control in contact-rich settings (p.1).
- Existing use of haptics (p.1): often only a supplementary modality, not directly involved in the policy's action generation.
- Existing tactile-VLA work (p.9): concurrent FuSe (finetuning with auxiliary losses), ForceVLA (modality-specific routing), other vision-tactile-language policies. Specialized tactile policies usually lack language, which limits novel-instruction generalization and commonsense use (p.10).
- Claim (p.1, p.9): the VLM's prior knowledge already contains semantic understanding of physical interaction. Connecting it to tactile sensors with few demonstrations activates that knowledge for zero-shot generalization.
- Language should regulate how an action is done ("pick up the apple softly"), not only which action (p.2).
- Figure 1 (p.2): three capabilities. (a) Tactile-aware instruction following: low force for "insert USB gently" learned, then high force for "pull out charger hard" generalized. (b) Tactile-relevant common sense: firm grip for a known heavy iron ball, gentle grip for an unseen fragile pitaya. (c) Adaptive tactile-involved reasoning: wipe fails from low downward pressure on a new scenario, then the corrective action raises pressure. Very good motivation slide.

## 2. Method
Terminology:
- Action vector: augmented action that includes target position $P_{target}$ and target contact force $F_{target}$, taken from expert demos (p.4).
- Hybrid position-force controller: converts force targets into position adjustments (indirect, impedance-inspired) (p.4).
- Tactile-VLA-CoT: variant that generates a chain-of-thought about tactile feedback using the VLM's decoder (p.4-5).
- Normal vs shear force: tactile sensors capture both (p.5); the CoT text refers to them separately (p.4-5).

Pipeline (Fig. 2, p.3):
1. Encoders (p.3): pretrained ViT (as in pi0) for the last H frames; common language tokenizer; a simple MLP tactile encoder compresses the concatenated history of H tactile measurements into one fused token; proprioceptive state also input (Fig. 2).
2. Token-level fusion (p.3): visual, language and tactile tokens are concatenated into a prefix $S_t$ and processed by a pretrained VLM (Gemma 2.6B) trunk with non-causal attention (Fig. 2).
3. Tactile-aware action expert (300M per Fig. 2) outputs target position and target force (p.4).
4. Training (p.4): initialize shared parts from pi0; tactile encoder and modified action expert are random-init. End-to-end finetuning with a Conditional Flow Matching loss penalizing both kinematic and force dimensions.
5. Hybrid controller (p.4): $\Delta F=F_{target}-F_{measured}$; position command is adjusted only if $\|\Delta F\|>\tau$; a PID controller tracks the result. Gripper Cartesian position regulates net external force; gripper width independently regulates internal grasp force.
6. Tactile-VLA-CoT (p.4-5, Fig. 3): CoT triggered at fixed intervals. The model first judges success, then, if failed, analyzes force components (e.g., "grasping force is sufficient, but normal force is too low") and generates a new corrective instruction (e.g., "wipe the board again, but apply more downward force"). Fine-tuned with a small dataset of failure cases paired with language annotations of the cause; this also mitigates catastrophic forgetting.
7. Data collection (p.5, Fig. 4): UMI handheld gripper augmented with dual high-resolution tactile sensors (normal + shear) and a GoPro camera, 3D-printed gripper; operators feel contact force. 100 Hz tactile, 20 Hz vision, timestamps aligned, tactile down-sampled to frames.

Difference from existing methods: force is part of the action space and is controlled (not only an extra input); language modifiers map to force magnitudes; tactile feedback drives explicit reasoning (p.2, p.9).

Key equations:
- Prefix (p.3, Eq. 1): $S_t=[E'_{vis}(I_{t-H+1}),\dots,E'_{vis}(I_t),\,E_{lang}(L_t),\,E'_\psi([T_{t-H+1},\dots,T_t])]$
- Hybrid controller (p.4, Eq. 2): $P_{hybrid}=P_{target}+K\cdot\Delta F$ if $\|\Delta F\|>\tau$, else the correction term is 0 (K = gain matrix).

Pseudocode (my paraphrase, not in the paper):
```
S = [ViT(I_{t-H+1..t}), Tok(L), MLP(T_{t-H+1..t})]
(P_tgt, F_tgt) = ActionExpert(VLM(S))      # flow matching
dF = F_tgt - F_meas
P_cmd = P_tgt + K*dF if |dF| > tau else P_tgt
PID tracks P_cmd
(CoT variant) every N steps: VLM judges success -> if fail, diagnose -> new instruction
```

## 3. Experiments
- Baselines (p.5-6): pi0-base, pi0-fast; proposed Tactile-VLA and Tactile-VLA-CoT.
- Hardware/sensors: UMI-based tactile gripper for data collection (p.5). Robot arm used for deployment, tactile sensor model, and number of eval trials for Table 1/2/4: not stated.
- Research questions (p.5): RQ1 force-language generalization, RQ2 commonsense force for unseen objects, RQ3 reasoning about physical failure.
- Tasks and data (p.6): (1) Charger/USB insertion and extraction: 100 demos "soft" and 100 "hard" USB, plus 100 charger demos (motion only, no force words). (2) Tabletop grasping: 50 demos per object, 6 seen objects in training plus 6 unseen for eval (Fig. 5b). (3) Wiping the board: 100 successful and 100 failed whiteboard demos; no blackboard in training.
- RQ1 results (p.6-7):
  - Table 1 success rates, USB / Charger: pi0-base 5 / 40, pi0-fast 0 / 25, Tactile-VLA 35 / 90.
  - Table 2 applied force (N). Learned task USB: Tactile-VLA "softly" 0.51, "hard" 2.57; unseen adverbs "gently" 0.75, "firmly" 1.98, "rigidly" 2.42, "harder" 2.94 (above trained "hard"). Zero-shot Charger: "softly" 4.68, "hard" 9.13. pi0 and pi0-fast show forces about 2.3-2.8 N on USB regardless of word (and 5.7-7.4 N on charger), no correlation with the adverb.
- RQ2 results (p.7-8): Table 3, grasp success without deformation, 10 trials per object. Tactile-VLA 80-100% on all 12 objects (e.g., Pitaya 90, Melon 80, BlueBerry 100, PaperBox 90 for fragile OOD). pi0-base on fragile OOD: Melon 0, BlueBerry 0, PaperBox 0 (Pitaya 50); pi0-fast: Pitaya 40, Melon 10, 0, 0. Fig. 6 (p.8): mean applied force over 5 trials shows hard/medium/soft for heavy/light/fragile classes, including OOD.
- RQ3 results (p.8-9): Table 4, whiteboard (ID) / blackboard (OOD): pi0-base 40 / 0, pi0-fast 45 / 0, Tactile-VLA 80 / 15, Tactile-VLA-CoT 75 / 80. On the blackboard, default force 3.5 N failed; CoT raised it to 6.7 N (34% above the 5 N in whiteboard training data) and succeeded. Fig. 7 (p.9) shows the three stages.
- Takeaway (p.9-10): tactile fusion plus VLM priors yields zero-shot force-language grounding, commonsense force, and reasoning-based force adaptation from few demos.
- Limitations: no limitations section. Observed in the text only: Tactile-VLA without CoT reaches just 15% on blackboard (Table 4); absolute success is modest on USB (35%) (Table 1). Few tasks (three), 20-trial-scale evaluations (trial counts for some tables not stated).
- Best figures for slides: Fig. 1 (p.2) motivation/results overview; Fig. 2 (p.3) architecture; Fig. 3 (p.4) CoT dialogue; Table 2 (p.7) force values; Table 4 (p.9) + Fig. 7 (p.9) for reasoning; Fig. 4 (p.5) data collection.


---

# TacVLA: Contact-Aware Tactile Fusion for Robust Vision-Language-Action Manipulation

Note: text extracted with PyMuPDF (Read tool could not render pages); all 8 body pages plus references were read as text. Figure contents are inferred from text/captions only.

## 0. Paper Information
- Title: TacVLA: Contact-Aware Tactile Fusion for Robust Vision-Language-Action Manipulation (p.1)
- Authors: Kaidi Zhang*, Heng Zhang*, Zhengtong Xu, Zhiyuan Zhang, Md Rakibul Islam Prince, Xiang Li, Xiaojing Han, Yuhao Zhou, Arash Ajoudani, Yu She (*equal contribution) (p.1)
- Affiliations (p.1): Istituto Italiano di Tecnologia (HRI2 Lab); University of Genoa (DRIM PhD); Purdue University (Edwardson School of Industrial Engineering; Elmore Family School of ECE).
- Venue/year: arXiv preprint, arXiv:2603.12665v4 [cs.RO], 6 Sep 2026 (p.1). Peer-reviewed venue: not stated.
- Paper URL: https://arxiv.org/abs/2603.12665 (derived from the arXiv ID on p.1)
- Project URL: https://sites.google.com/view/tacvla (videos; p.2)
- Code URL: not stated in the PDF.
- Summary (<=40 words): TacVLA fine-tunes pi0.5 with a compact 15x8 tactile array tokenized into 36 tokens. A contact-aware gate masks tactile tokens when no contact is detected. It averages 83.75% on disassembly, 70% on in-box picking, and is more robust to occlusion.

## 1. Motivation
- VLAs rely mostly on vision and language, which becomes a bottleneck under severe occlusion (end-effector or object blocks the camera) and in fine-grained contact-rich tasks where vision misses contact force, friction, and surface texture (p.1-2).
- Limitations of existing tactile VLAs (p.2-3):
  - Evaluations rarely stress sustained, precise contact tasks such as disassembly; robustness under occlusion, human perturbation, and dynamic contact is underexplored.
  - Many treat tactile data as image-like input (refs [10],[11],[13],[14]), which increases token length and compute.
  - Simply concatenating tactile tokens can interact poorly with other tokens, especially in non-contact phases; most approaches use static fusion assuming tactile is always present (p.3).
- Key question (p.2): how should tactile be integrated into VLA architectures for robust fine-grained manipulation?
- Figure 1 (p.1): overview. (a) inputs (front + wrist RGB, language, tactile); (b) architecture with SigLIP, tokenizer, tactile MLP encoder, Gemma 2.6B VLM, action expert; (c) contact-aware gating (tactile map -> tokens -> mask by contact); (d) tasks plus robustness tests (camera blocked, human disturbance). Best single slide for method plus tasks.

## 2. Method
Terminology:
- Taxel: one element of the tactile array; the 15x8 array gives 120 normal-force readings (p.5).
- Contact flag $c_t\in\{0,1\}$: 1 when the number of taxels over a pressure threshold exceeds a fixed count (threshold-based) (p.4).
- Contact-aware gating: mask and zero tactile tokens when $c_t=0$ while keeping a fixed token layout (p.4).

Pipeline (Fig. 1b, p.1; Sec. III, p.4):
1. Tokenize (p.4): front and wrist images via SigLIP; language plus robot proprioception via the PaliGemma tokenizer; tactile 15x8 map via lightweight MLP into 36 tactile tokens, plus fixed 2D sine-cosine positional embeddings.
2. Gate (p.4): contact flag decides whether tactile tokens participate; tokens are not inserted/removed, only masked and zeroed.
3. Concatenate $\tilde z_t=[z^{vis}_t,z^{lan+pro}_t,\tilde z^{tac}_t]$ and feed to a pretrained VLM with non-causal attention over the prefix (p.4).
4. Action expert following the pi0.5 design, trained with flow matching, predicts an action chunk $a_{t:t+H}\sim\pi_\theta(\cdot\mid\tilde z_t)$ (p.4).
5. Training (p.4-5): task-specific models; LoRA on the OpenPI pi05 base checkpoint; 10,000 gradient steps; tactile encoder trained jointly.

Why gating (p.4): tactile is uninformative in non-contact phases and adds attention tokens; zeroing also suppresses embedding/positional offsets and pre-contact sensor noise or drift.

Difference from existing methods (p.2-3, p.4): low-dimensional tactile tokens instead of image-like input, and state-dependent token-level modality arbitration (contact-gated) instead of static always-on fusion.

Key equations (p.4):
- $M^{tac}_t = c_t\cdot\mathbf 1$ (tactile submatrix of the attention mask)
- $\tilde z^{tac}_t = c_t\cdot z^{tac}_t$

Pseudocode (my paraphrase):
```
c_t = 1 if (#taxels > p_thresh) >= N_min else 0
z_tac = MLP(tactile_15x8) + pos_emb          # 36 tokens
z_tac, mask = c_t * z_tac, c_t * ones        # off when no contact
a_{t:t+H} = ActionExpert(VLM([z_vis, z_lang+prop, z_tac], mask))
```

## 3. Experiments
- Baselines (p.5): Finetuned Pi0.5 (vision-language only); Diffusion Policy + Tactile; 3D Diffusion Policy + Tactile (diffusion baselines take multi-view images plus the same tactile input, trained 200 epochs, features concatenated). Ablation: Finetuned Pi0.5 + Tactile (w/o gating) (p.7).
- Hardware (p.5, Fig. 2): 7-DoF Franka Emika Panda with parallel gripper; fixed front camera and wrist camera; tactile array sensor adapted from 3D-ViTac [32] on one finger, 15x8 normal-force map (120 taxels); no shear information stated.
- Tasks (p.4-5, Fig. 3-4, Table I): four "constraint-locked" disassembly tasks (tight shaft, press clip, shaft rotation 90 deg, slide pull) and in-box picking (front camera sees no box inside; wrist camera has limited light and occlusion). 50 teleoperated demos per task, 10 Hz recording; separate model per task. 20 trials per task evaluated.
- Main results (Table II, p.5), success out of 20:
  - Disassembly Task 1-4 / average / in-box: 3D DP+Tactile 40, 40, 45, 0 / 31.25% / 5%; DP+Tactile 70, 70, 30, 25 / 48.75% / 0%; Finetuned Pi0.5 80, 80, 65, 30 / 63.75% / 10%; TacVLA 100, 90, 70, 75 / 83.75% / 70%.
  - Largest gain over Pi0.5: Task 4 (+45 points). Pi0.5 gets stuck and repeatedly regrasps; TacVLA moves to lifting only after contact is physically detected (p.5-6).
  - Abstract claim (p.1): about 20% average gain in disassembly and 60% in in-box picking (consistent with Table II vs Pi0.5), 2.1x improvement under visual occlusion.
- Robustness (p.6-7):
  - Front camera blocked, Fig. 6: Pi0.5 40, 40, 5, 35% on Tasks 1-4; TacVLA 70, 65, 45, 70%; average about 30% to over 60%. Largest gain Task 3 (+40).
  - Human disturbance (qualitative, Fig. 5b): object is moved back into the box mid-lift; TacVLA re-localizes and regrasps; Pi0.5 fails to recover. No numbers.
- Ablation (Table III, p.7): w/o gating: disassembly 90, 80, 60, 55 (avg 71.25%), in-box 40%, vs TacVLA 83.75% and 70%. On Task 3 no-gating (60%) is below non-tactile Pi0.5 (65%). Under blocked camera, no-gating loses about 25% on average vs TacVLA. Fig. 7 failure cases: misalignment, regrasp, stuck, failed lift.
- Takeaway: tactile helps most when vision is degraded, and naive always-on tactile fusion can hurt; gating by contact state is key.
- Limitations stated (p.7-8): (1) gating is a binary threshold heuristic, not learnable or gradual; (2) tactile sensor has limited spatial resolution; (3) only short-horizon, contact-centric tasks; long-horizon extension is future work. Also observable: separate model per task, 50 demos per task, single robot and sensor, human-disturbance test is qualitative only.
- Best figures for slides: Fig. 1 (p.1) overview; Fig. 3/4 (p.2-3) task procedures; Table II (p.5); Fig. 6 (p.6) blocked-camera bars; Table III + Fig. 7 (p.7) gating ablation.


---

