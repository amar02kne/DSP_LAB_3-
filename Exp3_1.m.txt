% Load the piano audio file
[piano_audio, sample_rate] = audioread('Opera.wav');
% Play the audio
sound(piano_audio, sample_rate)
% Load the audio files
[piano_audio, piano_sample_rate] = audioread('piano2.wav');
[flute_audio, flute_sample_rate] = audioread('flute4.wav');

% Calculate the fundamental frequency for pianoα.wav
fundamental_piano = calculateFundamentalFrequency(piano_audio, piano_sample_rate);

% Calculate the fundamental frequency for fluteα.wav
fundamental_flute = calculateFundamentalFrequency(flute_audio, flute_sample_rate);

% Calculate the ratio (β) between the fundamental frequencies
beta = fundamental_flute / fundamental_piano;

% Display the β value
fprintf('The value of β for "flute2.wav" relative to "piano2.wav" is approximately %.4f\n', beta);

% Function to calculate the fundamental frequency
function fundamental = calculateFundamentalFrequency(audio_signal, sample_rate)
    % Apply FFT to the audio signal
    fft_result = fft(audio_signal);
    
    % Calculate the magnitude spectrum
    magnitude_spectrum = abs(fft_result);
    
    % Calculate the corresponding frequency values
    num_samples = length(audio_signal);
    frequency_values = (0:num_samples-1) * (sample_rate / num_samples);
    
    % Find the index of the peak (fundamental harmonic)
    [~, peak_index] = max(magnitude_spectrum);
    
    % Get the corresponding frequency value
    fundamental = frequency_values(peak_index);
end

