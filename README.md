# Image-Processing
Source Code: Xử lý hình ảnh
clearvars; clc;
% Đọc giá trị file jpg
O_image = imread('anh.jpg');
% Hiển thị hình gốc
figure;
subplot(2, 2, 1), imshow(O_image), title('Hình gốc');
% Tỉ lệ lấy mẫu
sampling_rate = 4; % Điều chỉnh nếu cần, càng thấp càng đỡ xấu
% Lấy mẫu hình ảnh
S_image = O_image(1:sampling_rate:end, 1:sampling_rate:end, :); % Lấy mẫu từ các hàng và cột của ảnh với bước nhảy = sampling rate
subplot(2, 2, 2), imshow(S_image), title('Hình sau lấy mẫu');
% Lượng tử hình ảnh
Q_image = im2uint8(S_image); % Chuyển đổi thành hình 8-bit
subplot(2, 2, 3), imshow(Q  _image), title('Hình sau lượng tử');
% Khôi phục hình ảnh bằng phương pháp nội suy tuyến tính tam giác
R_image = imresize(Q_image, size(O_image, 1:2), 'bicubic');
subplot(2, 2, 4), imshow(R_image), title('Hình sau khôi phục');
% Hiển thị cửa sổ so sánh O_image và R_image
figure;
imshowpair(O_image, R_image, 'montage');
title('So sánh hình gốc và hình khôi phục');

#Sound-Prcocessing
Source code
clc
% Đọc file âm thanh
audio_info = audioinfo('joker.mp3');    % Lấy thông tin của file âm thanh
audioDefault = audioread('joker.mp3');  % Đọc dữ liệu từ file âm thanh
fs = audio_info.SampleRate;             % Lấy tần số lấy mẫu của file âm thanh
duration = audio_info.Duration;         % Lấy thời lượng của file âm thanh
% Vẽ đồ thị âm thanh
time = (0:length(audioDefault)-1) / fs;    % Tạo mảng thời gian tương ứng với mỗi mẫu âm thanh
figure;
plot(time, audioDefault);
xlabel('Thời gian (giây)');
ylabel('Amplitude');
title('Đồ thị âm thanh');
% Hiển thị thông tin về thời gian và tần số của file âm thanh
fprintf('Thời gian của file âm thanh là %.2f giây.\n', duration);
fprintf('Tần số lấy mẫu của file âm thanh là %.2f Hz.\n', fs);
%% Sample
fsNew =  20000;                                     % Tần số lấy mẫu mới
audioSampled = resample(audioDefault, fsNew, fs);      % Thay đổi tần số lấy mẫu
audiowrite('jokerSample.wav', audioSampled, fsNew); % Ghi dữ liệu âm thanh sau lấy mẫu vào file âm thanh mới    
% Vẽ đồ thị âm thanh mới có tần số lấy mẫu mới
timeResampled = (0:length(audioSampled)-1) / fsNew;
figure;
plot(timeResampled, audioSampled);
xlabel('Thời gian (giây)');
ylabel('Amplitude');
title('Đồ thị âm thanh với tần số lấy mẫu mới');
% Hiển thị thông tin về thời gian và tần số mới của file âm thanh
durationResampled = length(audioSampled) / fsNew; % Tính thời lượng mới của âm thanh
fprintf('Thời gian của file âm thanh mới là %.2f giây.\n', durationResampled);
fprintf('Tần số lấy mẫu của file âm thanh mới là %.2f Hz.\n', fsNew);
%% Quantized
newBitDepth = 32;     % Số bit lượng tử hóa
audioQuantized = round(audioSampled * (2^(newBitDepth - 1))); % Lượng tử hóa và làm tròn lên  
audioQuantized = audioQuantized / 2^(newBitDepth - 1);
audiowrite('jokerQuantized.wav', audioQuantized, fsNew, 'BitsPerSample', newBitDepth); % Âm thanh sau khi lượng tử hóa
figure;
t = (0:length(audioQuantized)-1) / fsNew;
plot(t, audioQuantized);
xlabel('Thời gian (s)');
ylabel('Biên độ');
title('Dữ liệu âm thanh sau khi lượng tử hóa');
fprintf("Số bit lượng tử hóa: %.1f \n", newBitDepth);
