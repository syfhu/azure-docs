---
title: What is Custom Voice? - Azure Cognitive Services | Microsoft Docs
description: This article overviews Microsoft Text to Speech voice customization, which enables you to create a recognizable, one-of-a-kind brand voice. 
services: cognitive-services
author: noellelacharite
manager: nolach
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
---
# Creating custom voice fonts

Microsoft Text to Speech (TTS) voice customization enables you to create a recognizable, one-of-a-kind voice for your brand: a *voice font.* 

To create your voice font, you make a studio recording and upload the associated scripts as the training data. The service then creates a unique voice model tuned to your recording. You can then use this voice font to synthesize speech. 

You can get started with a small amount of data for a proof of concept. But the more data you provide, the more natural and professional your voice sounds.

Voice customization is available for US English (en-US) and mainland Chinese (zh-CN).

## Prerequisites

The Text to Speech voice customization feature is currently in private preview. [Fill out the application form](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) to be considered for access.

You also need an Azure account and a subscription to the Speech service. [Create one] (https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started) if you haven't already. Connect your subscription to the Custom Voice portal as follows.

1. Log on to the [Custom Voice portal](https://customvoice.ai) using the same Microsoft account you used to apply for access.

2. Go to ‘Subscriptions’ under your account name on the top right.

    ![Subscriptions](media/custom-voice/subscriptions.png)

3. On the ‘Subscriptions’ page, choose ‘Connect existing subscription’.

     ![Connect existing subscription](media/custom-voice/connect-existing-sub.png)

4. Paste your subscription key into the table, as shown below. Each subscription has two keys and you may use either of them.

     ![Add Subscription](media/custom-voice/add-subscription.png)

You're ready to go!

## Prepare recordings and transcripts

A voice training dataset consists of a set of audio files, along with a text file containing the transcripts of all of these audio files.

You can prepare these files in either direction: either write a script and have it read by voice talent, or use publicly-available audio and transcribe them to text. In the latter case, edit disfluencies from the audio files, such as "um" and other filler sounds, stutters, mumbled words, or mispronunciations.

To produce a good voice font, it's important that the recordings are done in a quiet room with a high-quality microphone. Consistent volume, speaking rate, speaking pitch, and expressive mannerisms of speech are essential for building a great digital voice. To create a voice for production use, we recommend you use a professional recording studio and voice talent.

### Audio files

Each audio file should contain a single utterance (for example, a single sentence, or a single turn of a dialog system). All files must be in the same language (multilanguage custom voices are not supported). The audio files must also each have a unique numeric filename made with the filename extension `.wav`.

Audio files should be prepared as follows. Other formats are unsupported and will be rejected.

| **Property** | **Value** |
| ------------ | --------- |
| File Format  | RIFF (WAV)|
| Sampling Rate| at least 16,000 Hz |
| Sample Format| PCM, 16-bit |
| File Name    | Numeric, with `.wav` extension |
| Archive Format| Zip      |
| Maximum Archive Size|200 MB|

Place the set of audio files into a single folder without subdirectories and package the entire set as a single ZIP file archive.

> [!NOTE]
> Wave files with a sampling rate lower than 16,000 Hz will be rejected. In the cases where a zip file contains waves with different sampling rates, only those equal to or higher than 16,000 Hz will be imported.
> The portal currently imports ZIP archives up to 200 MB. However, multiple archives may be uploaded. The maximum number of datasets allowed is 10 ZIP files for free subscription users, and 50 for standard subscription users.

### Transcripts

The transcription file is a plain text file (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE). Each line of the transcription file must have the name of an audio file, followed by a tab (code point 9) character, and finally its transcript. No blank lines are allowed.

For example:

```
0000000001	This is the waistline, and it's falling.
0000000002	We have trouble scoring.
0000000003	It was Janet Maslin.
```

The custom voice system normalizes transcripts by converting the text to lower-case and removing extraneous punctuation. It’s important that the transcripts are 100% accurate to the corresponding audio recordings.

> [!TIP]
> When building production Text-to-Speech voices, select utterances (or write scripts) considering both phonetic coverage and efficiency.

## Upload your datasets

After preparing your audio file archive and transcripts, upload them via the [Custom Voice Service Portal](https://customvoice.ai).

> [!NOTE]
> Datasets cannot be edited after they have been uploaded. If you forgot to include transcripts of some of the audio files, for example, or accidentally choose the wrong gender, you must upload the entire dataset again. Check your dataset and settings thoroughly before beginning the upload.

1. Sign in to the portal.

2. Choose **Data** under Custom Voice on the main page. 

    ![My Projects](media/custom-voice/my-projects.png)

    The My Voice Data table appears. It is empty if you have not yet uploaded any voice datasets.

3. Click **Import data** to open the page for uploading a new dataset.

    ![Import Voice Data](media/custom-voice/import-voice-data.png)

4. Enter a Name and Description in the provided fields. 

5. Select the locale for your voice fonts. Make sure the locale information matches the language of the recording data and the scripts. 

6. Select the gender of the speaker whose voice you are using.

7. Choose the script and audio files to upload. 

8. Click **Import** to upload your data. For larger datasets, importing may take several minutes.

> [!NOTE]
> Free subscription users can upload two datasets at a time. Standard subscription users can upload five datasets simultaneously. If you reach the limit, wait until at least one of your datasets finishes importing, then try again.

When the upload is complete, the My Voice Data table appears again. You should see an entry that corresponds to your just-uploaded dataset. 

Datasets are automatically validated after upload. Data validation includes a series of checks on the audio files to verify their file format, size, and sampling rate. Checks on the transcription files verify the file format and perform some text normalization. The utterances are transcribed using speech recognition, and the resulting text is compared with the transcript you provided.

![My Voice Data](media/custom-voice/my-voice-data.png)

The following table shows the processing states for imported datasets. 

| State | Meaning
| ----- | -------
| `NotStarted` | Your dataset has been received and is queued for processing
| `Running` | Your dataset is being validated
| `Succeeded` | Your dataset has been validated and may now be used to build a voice font

After validation is complete, you can see the total number of matched utterances for each of your datasets in the Utterance column.

You can download a report to check the pronunciation scores and the noise level for each of your recordings. The pronunciation score ranges from 0 to 100; a score below 70 normally indicates a speech error or script mismatch. A heavy accent can reduce your pronunciation score and impact the generated digital voice.

A higher signal-to-noise ratio (SNR) indicates lower noise in your audio. You can typically reach a 50+ SNR by recording through professional studios. Audio with an SNR below 20 can result in obvious noise in your generated voice.

Consider re-recording any utterances with low pronunciation scores or poor signal-to-noise ratios. If re-recording is not possible, you might exclude those utterances from your dataset.

## Build your voice font

Once your dataset has been validated, you can use it to build your custom voice font. 

1. Choose **Models** in the “Custom Voice” drop-down menu. 
 
    The My Voice Fonts table appears, listing any custom voice fonts you have already created.

1. Click **Create voices** under the table title. 

    The page for creating a voice font appears. The current locale is shown in the first row of the table. Change the locale to create a voice in another language. The locale must be the same as the datasets being used to create the voice.

1. As you did when you uploaded your dataset, enter a name and description to help you identify this model. 

    The name you enter here will be the name you use to specify the voice in your request for speech synthesis as part of the SSML input, so choose carefully. Only letters, numbers, and a few punctuation characters like '-', '_' '(', ')' are allowed.

    A common use of the Description field is to record the names of the datasets that were used to create the model.

1. Choose the gender of your voice font. It must match the gender of the dataset.

1. Select the dataset(s) that you want to use for training the voice font. All datasets used must be from the same speaker.

1. Click **Create** to begin creating your voice font.

    ![Create Model](media/custom-voice/create-model.png)

Your new model appears in the My Voice Fonts table. 

![My Voice Fonts](media/custom-voice/my-voice-fonts.png)

The status shown reflects the process of converting your dataset to a voice font, as shown here.

| State | Meaning
| ----- | -------
| `NotStarted` | Your request for voice font creation is queued for processing
| `Running` | Your voice font is being created
| `Succeeded` | Your voice font has been created and may be deployed

Training time varies depending on the volume of audio data processed. Typical times range from about 30 minutes for hundreds of utterances to 40 hours for 20,000 utterances.

> [!NOTE]
> Free subscription users can train one voice font at a time. Standard subscription users can train three voices simultaneously. If you reach the limit, wait until at least one of your voice fonts finishes training, then try again.

## Test your voice font

Once your voice font is successfully built, you can test it before deploying it for use. Click **Test** in the Operations column. The test page appears for the selected voice font. The table is empty if you haven’t yet submitted any test requests for the voice.

![My Voice Fonts, part 2](media/custom-voice/my-voice-fonts2.png)

Click **Test with text** button under the table title to display a pop-up menu for submitting text requests. You can submit your test request in either plain text or SSML. The maximum input size is 1,024 characters, including all tags for SSML request. The language of your text must be the same as the language of your voice font.

![Voice Font Testing](media/custom-voice/voice-font-testing.png)

After filling in the text box and confirming the input mode, click **Yes** to submit your test request and return to the test page. The table now includes an entry that corresponds to your new request, and the now-familiar status column. It can take a few minutes to synthesize speech. When the status column reads Succeeded, you can download the text input (a `.txt` file) and audio output (a `.wav` file) and audition the latter for quality.

![Voice Font Testing, part 2](media/custom-voice/voice-font-testing2.png)

## Create and use a custom endpoint

After you have successfully created and tested your voice model, you deploy it in a custom Text to Speech endpoint. You then use this endpoint in place of the usual endpoint when making Text to Speech requests through the REST API. Your custom endpoint can be called only by the subscription that you used to deploy the font.

To create a new custom endpoint, choose **Endpoints** from the Custom Voice menu at the top of the page. The Deployment page appears, with its table of current custom voice endpoints, if any.

Click the **Deploy voices** button to create a new endpoint. In the Create Endpoint” page, the current locale is reflected in the first row of the table. To create a deployment for a different language, change the displayed locale. (It must match the voice you are deploying.) Enter the Name and Description of your custom endpoint.

From the Subscription menu, choose the subscription that you want to use. Free subscription users can have only one model deployed at a time. Standard subscription users can create up to 20 endpoints, each with its own custom voice.

![Create Endpoint](media/custom-voice/create-endpoint.png)

After selecting the model to be deployed, click **Create**. The Deployment page reappears, now with an entry for your new endpoint. It may take a few minutes to instantiate a new endpoint. When the status of the deployment is Succeeded, the endpoint is ready for use.

![My Deployed Voices](media/custom-voice/my-deployed-voices.png)

When the deployment status is Succeeded, the endpoint of your deployed voice font appears in the My deployed voices table. You can use this URI directly in an HTTP request.

Online testing of the endpoint is also available via the custom voice portal. To test your endpoint, choose **Endpoints testing** from the Custom Voice drop-down menu. The endpoint testing page appears. Choose a voice that you have deployed, and input the text to be spoken (in either plain text or SSML format) into the text box.

> [!NOTE] 
> When using SSML, the `<voice>` tag must specify the name you gave your custom voice when you created it.

Click **Play** to hear the text spoken in your custom voice font.

![Endpoint Testing](media/custom-voice/endpoint-testing.png)

The custom endpoint is functionally identical to the standard endpoint used for Text-to-Speech requests. See [REST API](rest-apis.md) for more information.

## Next steps

- [Get your Speech trial subscription](https://azure.microsoft.com/try/cognitive-services/)
- [Recognize speech in C#](quickstart-csharp-windows.md)
