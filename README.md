🌍 AeroSense: Spatiotemporal Pollution Tracing
AI-driven advective transport tracing of atmospheric pollutants in Delhi-NCR using Sentinel-5P TROPOMI and ERA5 reanalysis.

📺 Project Presentation & Defense
[Watch the AeroSense Video Defense Here] (Insert your YouTube/Google Drive link here)

📄 Abstract
Delhi-NCR consistently ranks among the most severely polluted urban agglomerations on Earth. Existing forecasting tools often rely on coarse-resolution chemical-transport models or sparse point-sensor data that fail to capture the full spatial extent of advecting pollution plumes.

Project AeroSense is a deep-learning forecasting engine that ingests an 8-year archive (2018–2025) of satellite-derived pollutant observations. The model processes a 7-day historical sequence to predict the complete multi-channel concentration field for the subsequent 24 hours on a 141 × 231 spatial lattice.

Ultimately, this research demonstrates that structural parsimony is a strategic necessity when modeling complex atmospheric dynamics from noisy satellite data. Through a systematic ablation study, the proposed custom Two-Gate Deep Conv-GRU significantly outperformed 101-layer ResNet architectures by filtering out satellite retrieval noise rather than memorizing it.

✨ Key Technical Innovations
Custom Deep Conv-GRU: Engineered a manually unrolled, two-gate convolutional recurrent layer. By reducing the parameter count by 25% relative to a standard Conv-LSTM, the model achieves a structural parsimony that naturally filters out TROPOMI retrieval artifacts while preserving advective momentum.

Memory Optimization: Engineered a half-precision (float16) DataCube. This compressed the 8-year multi-variable spatiotemporal tensor to a 1.1 GB in-memory footprint, eliminating I/O bottlenecks and enabling fully GPU-resident training on consumer hardware.

Multi-Channel Physical Integration: Modeled 6 concurrent channels—NO₂, CO, AOD, UVAI, and ERA5 Horizontal Wind Components (U, V)—to seamlessly couple emission signatures with fluid transport physics.

📂 Repository Structure
The project codebase is structured chronologically to reflect the research pipeline:

Plaintext
├── Phase_1_Data_Engineering/
│   ├── 01A_Sentinel5P_MODIS_Extraction.ipynb
│   ├── 01B_ERA5_Wind_Extraction.ipynb
│   ├── 01C_Spatial_Regridding.ipynb
│   └── 01D_Complete_Data_Preprocessing.ipynb
├── Phase_2_Model_Training/
│   ├── 02A_Custom_Deep_ConvGRU.ipynb         ⭐ [Proposed Model]
│   ├── 02B_Advanced_ConvLSTM.ipynb
│   ├── 02C_Standard_ConvLSTM.ipynb
│   ├── 02D_AlexNet_Spatial.ipynb
│   ├── 02E_ResNet50_Spatial.ipynb
│   ├── 02F_ResNet101_Spatial.ipynb
│   └── 02G_UNet_Spatial.ipynb
├── Phase_3_Evaluation_and_Visuals/
│   ├── 03A_Ablation_Study_Metrics.ipynb
│   └── 03B_AI_vs_Reality_Heatmaps.ipynb
├── data_assets/
│   └── Scaling_Parameters.json
└── Aerosense_Report.zip                      📦 [Full Research Archive]
🏆 Quantitative Ablation Results
Evaluated on a 20% chronological hold-out test set (~500 days). The proposed Custom Conv-GRU achieved state-of-the-art spatial regression performance.

Rank	Model Architecture	R² Score	MAE	MAPE
1	Custom Deep Conv-GRU (Proposed)	0.8305	0.001381	3.37%
2	Adv. ConvLSTM (Physics-Weighted)	0.7890	0.001638	4.06%
3	AlexNet (Spatial)	0.7200	0.001941	4.78%
4	ResNet-50 (Spatial)	0.6879	0.001952	4.62%
5	U-Net (Spatial)	0.6704	0.001996	4.74%
6	Standard ConvLSTM (Baseline)	0.6506	0.001925	4.47%
7	ResNet-101 (Spatial)	0.6323	0.002102	4.94%
Note: The monotonic degradation from ResNet-50 to ResNet-101 highlights the danger of over-parameterization under data scarcity (~2,000 training samples).

🗺️ Visual Performance (Prediction vs Reality)
(Visualizing the Day-8 predicted CO advection plume vs. TROPOMI ground truth)

⚙️ Requirements & Reproduction
To run the notebooks locally, ensure you have the following dependencies installed:

tensorflow >= 2.10

numpy

pandas

matplotlib

rasterio

Data Pipeline Note: Please run Phase_1_Data_Engineering/01D_Complete_Data_Preprocessing.ipynb to apply the NaN-healing logic and generate the required .npy DataCube before initializing model training.

Author: Yashaswi Garg

Institution: Bennett University (2024–2028)

Research Manuscript: The full documentation, including the technical report and high-resolution figures, is available in Aerosense_Report.zip.
