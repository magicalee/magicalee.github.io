---
layout: post
title: 'C#學習筆記 OpenCvSharp & Tesseract'
tags: ["C#"]
date: 2023-10-03 19:25
comments: true
categories: 
- "學習筆記"
---
# C#學習筆記 OpenCvSharp & Tesseract
應徵某外商時出的C#考題，要利用OpenCvSharp和Tesseract去實作讀取MP4檔案，並把其中的資料輸出成CSV。

## OpenCvSharp讀取影片
```c#
static void CaptureScreenshotsFromVideo(string videoFilePath, string outputFolder)
{
    using (var capture = new VideoCapture(videoFilePath))
    {
        if (!capture.IsOpened())
        {
            Console.WriteLine("Error: Could not open video file.");
            return;
        }

        Mat frame = new Mat();
        int frameNumber = 0;

        while (true)
        {
            capture.Read(frame);

            if (frame.Empty())
                break;

            // You can process the frame here or save it as an image.
            string outputPath = Path.Combine(outputFolder, $"frame_{frameNumber:D5}.png");
            Cv2.ImWrite(outputPath, frame);

            frameNumber++;
        }
    }
}
```
以上是最簡單利用OpenCvSharp去讀取影片，每個Frame都會擷取一張圖片儲存。
<!--more-->
## OpenCvSharp讀取影片有興趣的區塊(ROI)
但實際上影片中很多地方其實是跟要提取的資料毫不相關，這時我們就要定義Region of Interest(簡稱ROI)，也就是有興趣的區塊。底下是範例程式碼
```C#
static void Main(string[] args)
{
    string videoFilePath = "path_to_your_video.mp4";
    string outputFolder = "output_folder_for_screenshots";
    
    // Define the region of interest (ROI) as a Rect.
    Rect roi = new Rect(x, y, width, height); 

    Directory.CreateDirectory(outputFolder);

    CaptureScreenshotsAndExtractTextFromROI(videoFilePath, outputFolder, roi);
}

static void CaptureScreenshotsAndExtractTextFromROI(string videoFilePath, string outputFolder, Rect roi)
{
    using (var capture = new VideoCapture(videoFilePath))
    {
        if (!capture.IsOpened())
        {
            Console.WriteLine("Error: Could not open video file.");
            return;
        }

        Mat frame = new Mat();
        int frameNumber = 0;

        while (true)
        {
            capture.Read(frame);

            if (frame.Empty())
                break;

            // Crop the frame to the specified ROI.
            Mat roiMat = new Mat(frame, roi);

            // Save the ROI as an image for reference (optional).
            string outputPath = Path.Combine(outputFolder, $"roi_frame_{frameNumber:D5}.png");
            Cv2.ImWrite(outputPath, roiMat);

            frameNumber++;
        }
    }
}

```
## Tesseract文字辨識
成功提取ROI後，以Tesseract去做文字判讀，範例程式碼如下
```C#
static string ExtractTextFromImage(string imagePath)
{
    using (var engine = new TesseractEngine(@"path_to_tesseract_data_folder", "eng", EngineMode.Default))
    using (var image = Pix.LoadFromFile(imagePath))
    {
        using (var page = engine.Process(image))
        {
            return page.GetText();
        }
    }
}
```
## 影像預處理
提取的ROIs中，可能文字對比沒有那麼明顯、或文字過小，這些都會造成Tesseract辨識的準確度下降，這時就需要先對這些ROI處裡後，再給Tesseract辨識。底下範例的預處理為放大後做灰階，最後用Thresholding轉換成黑白。
```C#
static string ExtractTextFromImage(string imagePath)
{
    using (var engine = new TesseractEngine(@"path_to_tesseract_data_folder", "eng", EngineMode.Default))
    using (var image = Pix.LoadFromFile(imagePath))
    {
        var resizedImage = new Mat();

        //Resize
        Cv2.Resize(image, resizedImage, new OpenCvSharp.Size(image.Width * 2.5, image.Height * 2.5));       
            
        //Gray Scale
        using var imgGray = new Mat();
        Cv2.CvtColor(resizedImage, imgGray, ColorConversionCodes.BGR2GRAY);

        //Black and White
        using var imgThreshold = new Mat();
        Cv2.Threshold(imgGray, imgThreshold, 0, 255, ThresholdTypes.Binary | ThresholdTypes.Otsu);

        using (var page = engine.Process(imgThreshold))
        {
            return page.GetText();
        }
    }
}

```
## 邏輯與格式判斷
即使已預處理圖片，許多人名還是會辨識錯誤，這時只能從最後輸出的邏輯去處理。看是否可以偵測到合理的格式才輸出，但可能永遠都辨識不出合理的格式，導致其他資料也無法準確紀錄。

## 結語
這面試題我蠻喜歡的，比傳統筆試題或演算法題還有挑戰性，也學到不少。