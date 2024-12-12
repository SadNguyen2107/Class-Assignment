

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
2. MFCCs
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
3. Model
   1. 
4. 