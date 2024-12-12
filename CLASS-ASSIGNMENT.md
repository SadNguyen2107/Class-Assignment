

# CLASS ASSIGNMENT
1. System architecture (propose mode)
    * Draw the proposed system architecture (propose mode)
    * Describe components in details
    * Write steps of algorithm / propose model in codes or maths
2. Development of AI model application
    * Give problems based on the system architecture (given by 1) real world
    * Develop an application real-world case study
3.	Discuss the limitations and advantages of your AI project

## 1. System Architecture:
![System Workflow](./Images/workflow.jpg)

1. Audio data
   1. Audio File
   2. Data in bytes
   3. Encoding to 16bit, sample rate 16000
2. MFCCs
### **2. Steps to Compute MFCCs**

1. **Pre-emphasis**:
   - A pre-emphasis filter is applied to the audio signal to boost the energy of high frequencies. This compensates for the natural attenuation of high-frequency sounds during recording.
   - Example filter: $y(t) = x(t) - \alpha \cdot x(t-1)$, where $\alpha$ is usually 0.95.

2. **Framing**:
   - The continuous audio signal is divided into overlapping short frames (e.g., 20-40 ms), since speech is quasi-stationary over short durations. A common overlap is 50%.
   - Each frame is processed independently.

3. **Windowing**:
   - A window function (e.g., Hamming window) is applied to each frame to minimize spectral leakage. This ensures that signal discontinuities at frame boundaries don’t introduce artifacts.
   - Example Hamming window: $w(n) = 0.54 - 0.46 \cdot \cos\left(\frac{2\pi n}{N-1}\right)$.

4. **Fast Fourier Transform (FFT)**:
   - The windowed frames are transformed into the frequency domain using the FFT to obtain the magnitude spectrum.

5. **Mel Filter Bank**:
   - The frequency domain signal is passed through a filter bank that consists of triangular filters spaced according to the mel scale.
   - The mel scale relates perceived pitch to frequency using the formula:
     $$
     \text{mel}(f) = 2595 \cdot \log_{10}\left(1 + \frac{f}{700}\right)
     $$
   - The filters emphasize frequencies important for human perception, discarding irrelevant high-frequency components.

6. **Logarithm**:
   - The output of the filter bank is converted to a logarithmic scale to mimic the logarithmic response of the human ear to sound intensity.

7. **Discrete Cosine Transform (DCT)**:
   - The log power spectrum is compressed by applying the DCT. This step decorrelates the features and concentrates energy into a few coefficients, reducing dimensionality.
   - The result is a set of coefficients called the Mel Frequency Cepstral Coefficients.

   ```python
   def CalculateMFCC(audioSignal, sampleRate):
        # Step 1: Pre-emphasis
        # Apply pre-emphasis filter to boost higher frequencies
        signal = audioSignal[n] - (alpha * audioSignal[n-1])
        # typically alpha = 0.95

        # Step 2: Framing
        # Split signal into overlapping frames
        frameLength = 25ms  # typical value
        frameShift = 10ms   # typical value
        frames = split_into_overlapping_frames(signal, frameLength, frameShift)

        # Step 3: Windowing
        # Apply Hamming window to each frame to reduce spectral leakage
        For each frame:
            frame = frame * hamming_window(frameLength)

        # Step 4: FFT
        # Compute Fast Fourier Transform for each frame
        For each frame:
            spectrum = FFT(frame)
            powerSpectrum = |spectrum|^2

        # Step 5: Mel Filter Bank
        # Create triangular mel-scale filter bank
        numFilters = 26  # typical value
        melFilters = create_mel_filterbank(numFilters, sampleRate, fftSize)
        
        # Apply mel filterbank
        For each frame:
            melSpectrum = apply_filterbank(powerSpectrum, melFilters)

        # Step 6: Log compression
        # Take logarithm of mel spectrum
        logMelSpectrum = log(melSpectrum)

        # Step 7: DCT (Discrete Cosine Transform)
        # Apply DCT to get cepstral coefficients
        numCoefficients = 13  # typical value
        mfcc = DCT(logMelSpectrum)[0:numCoefficients]

        # Optional Step 8: Liftering
        # Apply liftering to smooth coefficients
        mfcc = lifter(mfcc)

        Return mfcc

    def create_mel_filterbank(numFilters, sampleRate, fftSize):
        # Convert frequency range to mel scale
        lowFreq = 0
        highFreq = sampleRate/2
        lowMel = hz_to_mel(lowFreq)
        highMel = hz_to_mel(highFreq)
        
        # Create equally spaced points in mel scale
        melPoints = linear_space(lowMel, highMel, numFilters + 2)
        
        # Convert back to Hz
        freqPoints = mel_to_hz(melPoints)
        
        # Create triangular filters
        For i from 1 to numFilters:
            Create triangular filter centered at freqPoints[i]
            with edges at freqPoints[i-1] and freqPoints[i+1]
        
        Return filters

    def hz_to_mel(frequency):
        Return 2595 * log10(1 + frequency/700)

    def mel_to_hz(mel):
        Return 700 * (10^(mel/2595) - 1)
   ```
8. Model
   
    LSTM + CNN:
    ![LSTM + CNN ](./Images/lstm.png)



---

### **1. Why MFCCs Are Useful**
Speech signals are highly variable, influenced by the speaker, recording environment, and microphone. MFCCs help by capturing the most important features of speech while discarding unnecessary information like noise. They are derived from the mel scale, which mimics the way humans perceive sound, making them particularly suited for applications like speech recognition.

---



---

### **3. Interpretation of MFCCs**
- The first coefficient, often called the **0th MFCC**, represents the average log energy of the signal (akin to loudness).
- Higher-order coefficients capture finer spectral details. Typically, the first 12-13 coefficients are used for most applications, as they convey the majority of the speech signal's information.




-- part 2: real life problem
---

### **5. Applications**
- **Speech Recognition**: Recognizing spoken words or phrases.
- **Speaker Recognition**: Identifying or verifying a speaker's identity.
- **Music Analysis**: Classifying genres or detecting beats.
- **Environmental Sound Recognition**: Identifying ambient sounds like alarms or nature sounds.


- Idea of an application:
-  

-- part 3: advantages, limitation
---

6. Limitations
Not Robust to Noise: MFCCs can be sensitive to background noise, requiring preprocessing like noise reduction.
Assumes Stationarity: MFCCs work well for quasi-stationary segments but might struggle with rapidly changing signals.

4. Advantages of MFCCs
Perceptual Basis: The mel scale aligns with human auditory perception.
Compact Representation: MFCCs distill complex spectral information into a small set of coefficients.
Effective for Speech Analysis: They emphasize features relevant to speech and de-emphasize irrelevant details like noise.
