---
title: Introduction to Audio Data
date: 2023-12-25 18:00:00 +0900
categories: [Audio]
tags: [audio]
image: /assets/img/previews/2023-12-25-introduction-to-audio-data.png
---

## Introduction to Audio Data

소리는 주어진 시간에 무한한 값을 가진 연속적인 시그널입니다. 이러한 점은 디지털 기기에서 데이터를 다루는데 문제를 야기합니다. 디지털 기기에 의해 데이터가 처리되고, 저장되고 전송되기 위하여, 이 연속 시그널은 비연속적인 값으로 변환되어야 하며, 이를 Digital Representation이라고 합니다.

음성과 음악과 같은 소리 데이터를 살펴보면 `.wav` `.mp3` `.flac` 과 같은 다양한 포맷을 발견할 수 있습니다. 이 포맷들은 이러한 Digital Representation, 즉 데이터의 압축을 어떻게 했는지에 따라 달라집니다.

실제 소리가 어떻게 Digital Representation이 되는지 살펴보겠습니다.
아날로그 신호가 처음에 마이크에 의해 수집되면, 이 신호는 아날로그-디지털 변환기(ADC)에 의해 디지털 신호로 변환됩니다. 이 신호는 일련의 숫자로 표현되며, 이 숫자는 특정 시간 간격으로 샘플링된 신호의 진폭을 나타냅니다. 이러한 샘플링된 신호는 디지털 신호로 표현되며, 이는 컴퓨터에서 저장하고 처리할 수 있는 형식입니다.


### Sampling Rate

샘플링 레이트는 초당 샘플링된 샘플의 수입니다. 샘플링 레이트가 높을수록, 샘플링된 신호는 원래 신호에 더 가깝게 됩니다. 샘플링 레이트는 Hz 단위로 표현됩니다. 1kHz의 샘플링 레이트는 1초에 1000개의 샘플을 의미합니다. 샘플링 레이트가 높을수록, 샘플링된 신호는 원래 신호에 더 가깝게 됩니다.

샘플링 레이트의 선택은 주로 신호에서 캡처할 수 있는 최고 주파수를 결정합니다. 이를 나이퀴스트 한계라고도 하며 샘플링 레이트의 정확히 절반입니다. 사람 말의 가청 주파수는 8kHz 미만이므로 16kHz로 음성을 샘플링하는 것으로 충분합니다. 더 높은 샘플링 레이트를 사용한다고 해서 더 많은 정보를 캡처할 수 있는 것은 아니며, 단지 이러한 파일을 처리하는 데 드는 계산 비용만 증가하게 됩니다. 반면에 너무 낮은 샘플링 레이트로 오디오를 샘플링하면 정보 손실이 발생할 수 있습니다. 8kHz로 샘플링된 음성은 이 레이트에서는 더 높은 주파수를 캡처할 수 없기 때문에 음성이 뭉개져 들립니다.

데이터 세트에 있는 모든 오디오 예제의 샘플링 속도가 동일한지 확인하는 것은 중요합니다. 사용자 지정 오디오 데이터를 사용하여 사전 학습된 모델을 미세 조정하려는 경우, 데이터의 샘플링 레이트가 모델이 사전 학습된 데이터의 샘플링 레이트와 일치해야 합니다. 샘플링 레이트는 연속적인 오디오 샘플 사이의 시간 간격을 결정하며, 이는 오디오 데이터의 시간적 해상도에 영향을 미칩니다. 예를 들어 샘플링 레이트가 16,000Hz인 5초 사운드는 80,000개의 값으로 표현되는 반면, 샘플링 레이트가 8,000Hz인 동일한 5초 사운드는 40,000개의 값으로 표현됩니다.

각 오디오 샘플들의 길이와 샘플링 레이트가 다르기에, 이들을 활용하여 모델링 하기위해서는 일치시키는 작업이 필요합니다. 이를 _Resampling_ 이라고 합니다.


### Amplitude and bit depth

Sampling Rate가 1초에 얼마나 많은 샘플을 포함하느냐에 대한 정보였다면, Sample의 값 그 자체는 어떻게 표현될까요?

소리는 공기압의 변화로 발생합니다. 이 변화의 단위가 바로 데시벨로 표현됩니다. 우리는 이를 통해 소음을 판정합니다. 예로, 일반적인 대화는 60 dB 수준이며, 콘서트 장의 음악은 125 dB입니다.

Bit depth는 이렇게 기록된 amplitude가 얼마나 정밀한지를 의미합니다. 일반적으로 오디오에서 사용되는 bit depth는 16bit과 24bit 입니다. 16bit는 2^16 = 65536개의 값으로 표현됩니다. 24bit는 2^24 = 16777216개의 값으로 표현됩니다. 이는 16bit보다 더 많은 값으로 표현되기 때문에, 더 정확한 값으로 표현됩니다.


### Audio as a waveform

아마 오디오를  _waverform_ 으로 시각화한 것을 본 적이 있을 것입니다. 이는 시간에 따른 amplitude의 변화를 시각화한 것입니다.

이러한 유형의 시각화는 개별 사운드 이벤트의 타이밍, 신호의 전반적인 음량, 오디오에 존재하는 불규칙성이나 노이즈와 같은 오디오 신호의 특정 특징을 식별하는 데 유용합니다.

오디오 신호의 파형을 librosa라는 Python 라이브러리를 사용할 수 있습니다:

```python
import librosa

array, sampling_rate = librosa.load(librosa.ex("trumpet"))

import matplotlib.pyplot as plt
import librosa.display

plt.figure().set_figwidth(12)
librosa.display.waveshow(array, sr=sampling_rate)
```
![](/assets/audio/sorohanro_-_solo-trumpet-06.ogg)

![](/assets/img/previews/2023-12-25-introduction-to-audio-data.png)

이 그래프는 y축에 신호의 진폭을, x축에 시간을 표시합니다. 즉, 각 점은 이 사운드를 샘플링할 때 채취한 단일 샘플 값에 해당합니다.

오디오를 듣는 것과 함께 시각화하는 것은 작업 중인 데이터를 이해하는 데 유용한 도구가 될 수 있습니다. 신호의 모양을 보고, 패턴을 관찰하고, 노이즈나 왜곡을 발견하는 방법을 배울 수 있습니다. 정규화, 리샘플링 또는 필터링과 같은 방식으로 데이터를 전처리하면 전처리 단계가 예상대로 적용되었는지 시각적으로 확인할 수 있습니다. 모델을 학습시킨 후 오류가 발생한 샘플(예: 오디오 분류 작업)을 시각화하여 문제를 디버깅할 수도 있습니다.




### Frequency Spectrum

오디오를 시각화하는 다른 방법 중 하나로 _frequency spectrum_이 있습니다.
이는 DFT (discrete Fourier transoform)로 계산된 것으로, 특정 주파수에 대한 amplitude를 계산할 수 있습니다.

```python
import numpy as np

dff_input = array[:4096]

# calculate the discrete Fourier transform (DFT)
window = np.hanning(len(dff_input))
windowed_input = dff_input * window
dft = np.fft.fft(windowed_input)

# get the aplitude of spectrum in decibels (dB)
aplitute = np.abs(dft)
aplitute_db = librosa.amplitude_to_db(aplitute, ref=np.max)

# get the frequncy bins
frequency = librosa.fft_frequencies(sr=sampling_rate, n_fft=len(dff_input))

plt.figure().set_figwidth(12)
plt.plot(frequency, amplitude_db)
plt.xlabel("Frequency (Hz)")
plt.ylabel("Amplitude (dB)")
plt.xscale("log")
```

![](/assets/img/2023-12-25-introduction-to-audio-data_plot_2.png)


이 Plot은 오디오에 존재하는 다양한 주파수 성분의 강도를 표시합니다. 주파수 값은 일반적으로 로그 스케일로 표시되는 x축에 표시되고 진폭은 y축에 표시됩니다.


DFT의 출력은 실수 및 허수 성분으로 구성된 복소수 배열입니다. np.abs(dft)로 크기를 구하면 스펙트로그램에서 진폭 정보를 추출할 수 있습니다.
실수 성분과 허수 성분 사이의 각도는 소위 위상 스펙트럼을 제공하지만, 머신 러닝 애플리케이션에서 이 정보는 종종 버려집니다.

librosa.amplitude_to_db()를 사용하여 진폭 값을 데시벨 단위로 변환하면 스펙트럼의 세부 정보를 더 쉽게 볼 수 있습니다. 

오디오 신호의 Frequency Spectrum은 waveform과 정확히 동일한 정보를 포함하며, 동일한 데이터(여기서는 트럼펫 소리의 첫 번째 4096개 샘플)를 보는 두 가지 다른 방법일 뿐입니다.
waveform이 시간에 따른 오디오 신호의 진폭을 표시하는 반면, Frequency Spectrum은 고정된 시점에 개별 주파수의 진폭을 시각화합니다.



### Spectrogram

### Mel spectrogram

