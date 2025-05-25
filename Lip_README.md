
# 🎙️ Urdu Lip-Sync Research Dataset  
**Processed Audio-Visual Lectures from Virtual University**  


This dataset contains professionally processed Urdu-language lecture videos from Virtual University (VU), optimized for lip-synchronization and speech recognition research. The dataset features synchronized video frames and audio clips across Computer Science and Psychology domains.

## 📌 Dataset Highlights

- **Clean, curated content**: 5 hours of high-quality Urdu speech
- **Diverse speakers**: 3 male and 2 female native Urdu speakers
- **Research-ready**: Pre-processed with face cropping, language filtering, and artifact removal
- **Frame-optimized**: Intelligent frame differencing to capture meaningful lip movements

## 📊 Dataset Specifications

| Feature              | Specification                          |
|----------------------|---------------------------------------|
| Total Duration       | 5 hours (post-cleaning)               |
| Speakers             | 3 Male, 2 Female                     |
| Video Resolution     | 224×224 pixels @ 25 FPS               |
| Audio Format         | 16kHz WAV (2-second clips)            |
| Subject Domains      | Computer Science          |

## 🧑‍🏫 Speaker Profiles

### Male Lecturers
- **Computer Science**  
  📘 Introduction to Cloud Computing  
  📘 Introduction to Data Science  
  📘 Introduction to Web Engineering  

### Female Lecturers
- **Psychology**  
  🧠 Ethical Psychology  
  📊 Statistics in Psychology  

All speakers are native Urdu speakers with clear articulation, recorded in academic lecture settings.

## 🏗️ Dataset Structure

The dataset follows this organized structure:

```
dataset_root/
├── speaker_01/
│   ├── s1_01_001/
│   │   ├── 0.jpn-n.jpg
│   │   └── audio.wav
│   ├── s2_n_00n/
│   │   ├── 0.jpn-n.jpg
│   │   └── audio.wav
│   └── ...
├── speaker_02/
│   └── ...
└── ...
```

Each 2-second clip contains:
- `0.jpn-n.jpg`: Extracted video frame
- `audio.wav`: Corresponding audio segment

## 🧼 Data Cleaning Pipeline

### 1. Face Detection & Cropping
- Used state-of-the-art face detection to crop and align facial regions
- Standardized to 224×224 resolution for model compatibility

### 2. Language Filtering
- Removed segments containing non-Urdu speech
- Preserved only clean Urdu speech segments

### 3. Visual Quality Control
- Eliminated segments with:
  - Obstructed facial features (hands, microphones)
  - Unusual mouth movements (tongue visibility)
  - Poor lighting or focus

### 4. Temporal Segmentation
- Divided into 2-second clips for granular analysis
- Ensured audio-visual synchronization

## 🎞️ Frame Differencing Technology

### Intelligent Frame Selection
Our advanced frame differencing algorithm:

1. Converts frames to grayscale for efficient processing
2. Computes Mean Squared Error (MSE) between consecutive frames:
   ```
   MSE = Σ(Frameₜ - Frameₜ₋₁)² / Total_Pixels
   ```
3. Applies empirically-tuned threshold:
   - **Keep** if MSE > 4.0 (significant movement)
   - **Discard** if MSE ≤ 4.0 (redundant frames)

This ensures only meaningful lip movements are retained, reducing storage and computation needs.

## ⚙️ Setup & Preprocessing

## ⚙️ Preprocessing (For Fast Training)

To prepare the dataset for efficient training, convert your data using the following command:

```bash
python preprocess.py --data_root speaker1/ --preprocessed_root speaker1preprocessed/
```

Repeat this for each speaker directory. The preprocessed output will contain aligned and normalized frames and audio chunks.

---

## 🔧 Requirements

- ✅ **Python 3.6**
- ✅ **Librosa** for audio processing:
  ```bash
  pip install librosa==0.8.1
  ```
- ✅ **FFmpeg** for audio extraction:
  ```bash
  sudo apt-get install ffmpeg
  ```
- ✅ Other dependencies (defined in `requirements.txt`):
  ```bash
  pip install -r requirements.txt
  ```

> 💡 *You can also use the provided Docker image for isolated training environments.*

---

## 🔽 Pretrained Model Downloads

To run or fine-tune the Wav2Lip model, download the following pretrained checkpoints:

| Purpose                    | Command                                                                                                                                                        | Output Path                                             |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| 🎤 Wav2Lip Base            | `wget 'https://iiitaphyd-my.sharepoint.com/:u:/g/personal/radrabha_m_research_iiit_ac_in/Eb3LEzbfuKlJiR600lQWRxgBIY27JZg80f7V9jtMfbNDaQ?e=TBFBVW' -O '/kaggle/working/Wav2Lip/checkpoints/wav2lip.pth'` | `/Wav2Lip/checkpoints/wav2lip.pth`                      |
| 🔊 Wav2Lip GAN             | `wget 'https://iiitaphyd-my.sharepoint.com/personal/radrabha_m_research_iiit_ac_in/_layouts/15/download.aspx?share=EdjI7bZlgApMqsVoEUUXpLsBxqXbn5z8VTmoxp55YNDcIA' -O '/kaggle/working/Wav2Lip/checkpoints/wav2lip_gan.pth'` | `/Wav2Lip/checkpoints/wav2lip_gan.pth`                  |
| 🧠 Face Detection (SFD)    | `wget "https://www.adrianbulat.com/downloads/python-fan/s3fd-619a316812.pth" -O "/kaggle/working/Wav2Lip/face_detection/detection/sfd/s3fd.pth"`              | `/Wav2Lip/face_detection/detection/sfd/s3fd.pth`         |

---

## 🎯 Evaluation Metrics

### 📌 LSE-C and LSE-D

These metrics evaluate audiovisual synchronization quality.

#### Steps:

1. Clone the SyncNet repository:
   ```bash
   git clone https://github.com/joonson/syncnet_python.git
   ```

2. Install dependencies:
   ```bash
   cd syncnet_python
   pip install -r requirements.txt
   sh download_model.sh
   ```

3. Copy Wav2Lip evaluation scripts into SyncNet:
   ```bash
   cd Wav2Lip/evaluation/scores_LSE/
   cp *.py syncnet_python/
   cp *.sh syncnet_python/
   ```

4. Organize generated videos:
   ```
   /video_data_root/
   ├── video1.mp4
   ├── video2.mp4
   ...
   ```

5. Run the evaluation:
   ```bash
   cd syncnet_python
   sh calculate_scores_real_videos.sh /path/to/video_data_root
   ```

   Output is saved in `all_scores.txt`.

---

### 📌 FID (Fréchet Inception Distance)

Used to evaluate the quality of generated images.

1. Install:
   ```bash
   pip install pytorch-fid
   ```

2. Compute FID:
   ```bash
   python -m pytorch_fid path/to/realimages/ path/to/generatedimages/
   ```

3. GPU acceleration:
   ```bash
   python -m pytorch_fid --device cuda:0 path/to/real path/to/generated
   ```

---

## 🛠️ Troubleshooting

**Common Issues:**
1. *Face detection failures*: Verify SFD model is correctly placed
2. *Audio sync issues*: Check FFmpeg installation
3. *CUDA memory errors*: Reduce batch size

## 📜 License & Attribution
This dataset is available for **non-commercial research purposes only**. Please cite:
```
Virtual University Urdu Lip-Sync Dataset (2025)
```

## 📬 Contact
For dataset access or collaboration inquiries:
**Research Team**  
[iq67896tr@gmail.com]  
Virtual University AI Research Division  
