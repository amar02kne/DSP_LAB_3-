% Load the Opera.wav audio file
[opera_audio, sample_rate] = audioread('Opera.wav');

% Define the segment length (2000 samples)
segment_length = 20000;

% Calculate the number of segments
num_segments = floor(length(opera_audio) / segment_length);

% Define the number of figures (e.g., 10)
num_figures = 11;

% Calculate the step size to skip segments
step_size = max(1, floor(num_segments / num_figures));

% Loop through the segments and create figures
for i = 1:step_size:num_segments
    % Extract the current segment
    segment_start = (i - 1) * segment_length + 1;
    segment_end = min(i * segment_length, length(opera_audio));
    current_segment = opera_audio(segment_start:segment_end);

    % Compute the FFT for the segment
    fft_result = fft(current_segment);

    % Calculate the magnitude spectrum (in dB scale)
    magnitude_spectrum = 20 * log10(abs(fft_result));

    % Calculate the corresponding frequency values
    num_samples = length(current_segment);
    frequency_values = (0:num_samples-1) * (sample_rate / num_samples);

    % Plot the spectrum
    figure;
    plot(frequency_values, magnitude_spectrum);
    title(sprintf('Spectrum - Segment %d', i));
    xlabel('Frequency (Hz)');
    ylabel('Magnitude (dB)');
    grid on;
    
    % Limit the number of figures to 10
    if i >= num_figures * step_size
        break;
    end
end
[x, fs] = audioread('Opera.wav');

windowSize = 1024;  % Size of the analysis window
overlap = 512;     % Overlap between consecutive windows
nfft = 1024;        % Number of FFT points
[S, F, T] = spectrogram(x, windowSize, overlap, nfft, fs);

S_dB = 10 * log10(abs(S));

figure;
imagesc(T, F, S_dB);  % Display the spectrogram
axis xy;              % Invert the y-axis to have lower frequencies at the bottom
colormap jet;        % Set the colormap (you can choose different colormaps)
colorbar;            % Add a colorbar to the plot
title('Spectrogram of Opera.wav');
xlabel('Time (s)');
ylabel('Frequency (Hz)');
