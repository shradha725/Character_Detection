# Character_Detection
I = imread("q2.jpg");
figure(1);
imshow(I);
%converting rgb to gray
I1 = im2gray(I);
figure(2);
imshow(I1)

%run ocr(optical character recognition) on image
results = ocr(I1);
results.Text

% Set LayoutAnalysis to "Block" to instruct ocr to assume the image
% contains just one block of text.
results = ocr(I1,LayoutAnalysis="Block");
results.Text
BW = imbinarize(I1);
figure(3);
imshowpair(I1,BW,"montage")

% Remove keypad background.
Icorrected = imtophat(I1,strel("disk",15));
BW1 = imbinarize(Icorrected);
figure(4);
imshowpair(I1,BW1,"montage")

% Perform morphological reconstruction and show binarized image.
marker = imerode(Icorrected,strel("line",10,0));
Iclean = imreconstruct(marker,Icorrected);
Ibinary = imbinarize(Iclean);
figure(5);
imshowpair(Iclean,Ibinary,"montage")
BW2 = imcomplement(Ibinary);
figure(6);
imshowpair(Ibinary,BW2,"montage")
results = ocr(BW2,LayoutAnalysis="block");
results.Text

% Use the "CharacterSet" parameter to constrain OCR
results = ocr(BW2,CharacterSet="0123456789*#");
 results.Text

% Check if any text is recognized
if isempty(results.Text)
    disp("No characters found.")
else
    % Display the recognized text
    disp("Recognized text:");
    disp(results.Text);
end
