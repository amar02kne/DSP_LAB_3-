fs = 44100;  % Sample rate (you can adjust this)
duration = 3;  % Duration of the recording in seconds
recorder = audiorecorder(fs, 16, 1);  % 16-bit, mono
disp('Start whistling...');
record(recorder, duration);
pause(duration);  % Wait for the recording to finish
disp('Recording finished.');
whistle_audio = getaudiodata(recorder);
% Calculate the FFT for the recorded audio
% Calculate the FFT for the recorded audio
fft_result = fft(whistle_audio);

% Calculate the magnitude spectrum
magnitude_spectrum = abs(fft_result);

% Calculate the corresponding frequency values
num_samples = length(whistle_audio);
frequency_values = (0:num_samples-1) * (fs / num_samples);

% Find the index of the largest peak (fundamental harmonic)
[~, max_index] = max(magnitude_spectrum);

% Get the corresponding frequency value
fundamental_frequency = frequency_values(max_index);

% Plot the magnitude spectrum
figure;
plot(frequency_values, magnitude_spectrum);
title('Magnitude Spectrum');
xlabel('Frequency (Hz)');
ylabel('Magnitude');
grid on;

% Display the fundamental frequency
fprintf('The fundamental frequency of the whistling is approximately %.2f Hz.\n', fundamental_frequency);
% Whistle a tone that closely matches the reference frequency (e.g., 440 Hz) for 3 seconds.
fprintf('Test Case 1: Pass Scenario\n');
pause;
fundamental_frequency_reference = 440.0;  % Adjust to match the reference frequency
keylock();
fprintf('Expected Result: ACCESS GRANTED\n');
% Whistle a tone that does not match the reference frequency (e.g., 420 Hz) for 3 seconds.
fprintf('Test Case 2: Fail Scenario\n');
pause;
fundamental_frequency_reference = 440.0;  % Adjust to not match the reference frequency
keylock();
fprintf('Expected Result: ACCESS DENIED\n');

function keylock()
    % Record a 3-second audio clip
    fs = 44100;  % Sample rate (adjust as needed)
    duration = 3;  % Duration of the recording in seconds
    recorder = audiorecorder(fs, 16, 1);  % 16-bit, mono

    disp('Recording audio...');
    recordblocking(recorder, duration);
    disp('Recording finished.');

    % Retrieve the recorded audio data
    recorded_audio = getaudiodata(recorder);

    % Calculate the fundamental frequency of the recorded audio
    fundamental_recorded = calculateFundamentalFrequency(recorded_audio, fs);

    % Define the reference fundamental frequency
    reference_frequency = fundamental_frequency_reference;  % Use the value from Step 2

    % Set the tolerance (5% error)
    tolerance = 0.05 * reference_frequency;

    % Check if the recorded frequency matches the reference within tolerance
    if abs(fundamental_recorded - reference_frequency) <= tolerance
        disp('ACCESS GRANTED');
    else
        disp('ACCESS DENIED');
    end
end

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
    
    % Plot the magnitude spectrum with only the fundamental frequency
    figure;
    plot(frequency_values, magnitude_spectrum);
    hold on;
    plot(frequency_values(peak_index), magnitude_spectrum(peak_index), 'ro', 'MarkerSize', 10);
    title('Audio Spectrum with Fundamental Frequency Highlighted');
    xlabel('Frequency (Hz)');
    ylabel('Magnitude (dB)');
    grid on;
    legend('Magnitude Spectrum', 'Fundamental Frequency');
    hold off;
end
