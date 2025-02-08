# GAN-Generator-for-Geological-Facies-Modeling

This project shows how to build a Generative Adversarial Network (GAN) from scratch to generate synthetic geological facies models. It covers setting up the environment, preparing the data, building the model, training it, and evaluating the results.

---

## **1. Introduction**

Generative Adversarial Networks (GANs) consist of two core components:  
- **Generator (G):** Creates synthetic geological facies that closely resemble real examples.  
- **Discriminator (D):** Classifies inputs as real or generated, helping the Generator improve over time.  

By training these models on channelized reservoir data, we aim to synthesize realistic facies distributions which capture key spatial and morphological properties found in geological formations.

---

## **2. Project Outline**

1. **Setting Up the Environment**  
   Install the main dependencies (e.g., PyTorch, NumPy, and TensorFlow for TFRecords parsing) and configure the device (CPU/GPU).

2. **Data Preparation**  
   - Convert TFRecord files to NumPy arrays.  
   - Create a PyTorch `Dataset` and `DataLoader` for mini‐batch training.  

3. **Model Architecture**  
   - **Progressive Growing of GANs (ProGAN):** Starts at lower resolution (4×4) and progressively doubles the size until 64×64.  
   - **Generator:** Learned via progressive upsampling blocks, each stage adding detail to the facies geometry.  
   - **Discriminator:** Mirrors the Generator’s progression in reverse, downsampling from 64×64 back to 4×4.

4. **Training**  
   - Utilizes **WGAN-GP** loss to stabilize convergence.  
   - Implements a **fade‐in phase** for each resolution to smoothly integrate higher‐resolution layers.  
   - Tracks **Generator** and **Discriminator** losses, gradient penalties, and outputs via checkpoints.

5. **Evaluation**  
   - **Qualitative:** Visual inspection of generated facies (channels, variations, boundary continuity).  
   - **Quantitative:**  
     - **MS-SWD:** Multi‐Scale Sliced Wasserstein Distance for distribution similarity.  
     - **Variograms:** Comparing spatial continuity metrics between real and generated facies.  
     - **Diversity Checks:** Sliced Wasserstein distances and MDS to identify mode collapse or coverage gaps.

6. **Conclusions**  
   Summarizes key findings, highlighting areas of success in reproducing short‐range structures and pointing out further improvements for long‐range continuity and higher resolution.

---

## **3. Installation**

1. **Clone or Download**  
   ```bash
   git clone https://github.com/YourUsername/GAN-Generator-for-Geological-Facies-Modeling.git
   cd GAN-Generator-for-Geological-Facies-Modeling

2. **Install Dependencies**
   ```bash
   pip install torch torchvision numpy matplotlib seaborn tensorflow tqdm

3. **Check GPU**
    Ensure GPU availability for faster training; otherwise, the code can run on CPU with increased training time.

## **4. Usage**

1. **Data Setup**  
   - Place TFRecord and label files under a `Training_data/` directory (or update paths accordingly in the code).  
   - Run the provided scripts/notebooks to convert TFRecords into `.npy` format.

2. **Run Training**  
   - Execute `python GAN_Generator_for_Geological_Facies_Modeling.py`.  
   - The script will train progressively from 4×4 up to 64×64, saving checkpoints and logging losses at each stage.

3. **Generate Synthetic Facies**  
   - Load the final `generator_model.pth`, sample random latent vectors, and generate synthetic 64×64 facies.  
   - Modify resolution settings in the code to test intermediate resolutions if desired.

---

## **5. Results**

1. **Generated Samples (64×64)**  
   - Visual inspection reveals realistic channel structures with distinct contrasts and a variety of orientations, indicating successful learning of fundamental geological patterns.

2. **Variogram Comparisons**  
   - Real vs. generated variogram plots confirm the GAN effectively captures short‐range spatial correlation, with a slight underestimation at longer distances.

3. **SWD/MDS Analysis**  
   - The Sliced Wasserstein Distance (SWD) and MDS visualization show the generated samples’ distribution is near, but not fully overlapping, the real data cluster—suggesting moderate coverage of the dataset’s variability without complete duplication.

---

## **6. Conclusions**

Overall, the progressive growing GAN pipeline demonstrates a strong capacity for capturing channelized patterns, short‐range continuity, and facies contrasts. While the generated models closely align with real geological data in terms of visual appearance and variogram trends, there remains a small distribution gap in advanced metrics such as SWD and a slight underrepresentation of long‐range correlations.

1. **Channel Representation:** The model effectively reproduces channel geometry, orientation, and contrast.  
2. **Variogram Behavior:** Short‐distance correlations are learned accurately, though longer‐distance variance is somewhat underestimated.  
3. **Distribution Proximity:** SWD‐based evaluations indicate that fake and real distributions cluster closely but do not fully overlap.

---

## **7. Future Improvements**

In order to further refine the geological fidelity of the generated facies, several architectural and training‐related adjustments can be considered. These include more specialized GAN losses, domain‐specific metrics, and possibly 3D modeling for greater realism and applicability.

1. **Extended Training & Regularization:** Longer training at higher resolutions, combined with alternatives like R1 or non‐saturating logistic losses, may enhance fine‐scale details.  
2. **Advanced Architectures:** Incorporating StyleGAN2 or skip connections can preserve sharper facies boundaries and improve spatial continuity.  
3. **Domain‐Specific Evaluation & 3D Extensions:** Leveraging morphological metrics (channel connectivity, width distributions) and shifting to 3D data will better capture realistic geological complexity.

## **8. References**

- **[Paper]** Suihong Song *et al.* (2021). *Geological Facies modeling based on progressive growing of generative adversarial networks (GANs).* [Springer Link](https://link.springer.com/article/10.1007/s10596-021-10059-w)  
- **[Code]** [GitHub Repository](https://github.com/SuihongSong/GeoModeling_Unconditional_ProGAN/) by Suihong Song

---

## **9. License**

This project is distributed under the MIT License. See `LICENSE` for more details.
