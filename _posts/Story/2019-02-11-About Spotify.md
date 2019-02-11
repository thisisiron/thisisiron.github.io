---
title: "Spotify는 어떻게 나에 대해 잘 알까?"
categories: Story
---

Medium 포스팅을 읽으면서 끌리는 제목을 보게 되었다. "How Does Spotify Know You So Well?"라는 제목이었다. 일단 스포티파이는 여러 나라에서 사용되고 있는 음악 스트리밍 서비스이다. 물론 우리나라에서는 지원을 하지 않고 있어서 모를 수도 있지만 네이버에서 최근에 출시한 VIBE 앱과 유사하다. 네이버에서 아마 한국의 스포티파이를 만들고 싶을 정도로 비슷하다.

VIBE을 사용한 분들은 알겠지만 매일 자주 듣거나 좋아하는 아티스트의 노래를 담은 플레이리스트가 추천된다. 스포티파이에서도 마찬가지이다. 자주 듣던 노래와 유사한 노래들을 추천해주는 서비스이다.

머신러닝 공부를 하면서 이런 추천시스템은 어떻게 개발되었는지 궁금해서 읽게 되었다. 스포티파이는 다음 3가지 추천 모델을 사용한다고 한다.

1. **Collaborative Filtering models**
2. **Natural Language Processing (NLP) models**
3. **Audio models**

## Collaborative Filtering models

Collaborative Filtering 모델은 쉽게 생각하면 내가 사용하는 것과 다른 사람이 사용한 것을 기반으로 추천을 해준다. 예를 들어 나와 비슷한 음악을 들은 사용자가 좋았다고 평가한 음악을 내게 추천을 해준다. 스포티파이에서는 사용자의 플레이리스트에 저장된 노래 또는 노래를 듣고 해당 아티스트 페이지에 들어가는 데이터를 이용한다.

다음 그림은 해당 포스팅에 있던 이미지인데 collaborative filtering를 이해하기 좋은 이미지라 첨부했다.

<img src="/assets/images/collaborative_filtering.PNG"><br>

## NLP Models

NLP 모델은 노래의 추가 정보(아티스트, 프로듀서 등), 기사, 인터넷에 있는 텍스트 등을 분석하기 위해 사용된다. NLP란 자연어 처리를 의미하는데 자연어란 사람이 사용하는 언어이며 이것을 컴퓨터가 이해하도록 처리하는 것을 의미한다. 스포티파이가 어떻게 이러한 데이터를 모으는지 글쓴이도 구체적으로 모르지만 대략적인 설명을 해주었다.
 
매일 아티스트와 노래는 변화하는 수천 개의 top terms을 갖는다. 각각의 term은 상대적인 중요도와 연관된 weight을 가진다. 대략적으로 누군가 용어(term)을 가지고 아티스트와 노래에 대해 서술하는 확률로 생각하면 된다. 

이 NLP 모델은 term과 weight을 이용하여 두 곡이 비슷한지 결정해주는 벡터를 만든다. 

## Raw Audio Models

현재까지 소개한 모델로도 충분하지 않을까?라는 생각을 갖고 있지만 여기서 더 정확함을 더해주기 위해 사용된다. 예를 들어 스포티파이에 친구가 곡을 올렸다고 가정해보자. 근데 그 곡을 들은 사람은 50명 밖에 안 되며, 인터넷 어느 곳에서도 그 곡에 대해 언급하질 않는다면 위의 두 모델로는 충분하지 않을 것이다.

이 Audio 모델은 새로운 곡과 현재 인기있는 대중곡을 구별하지 못 하므로 데일리 추천곡 리스트에 포함될 수도 있다.

그럼 어떻게 이것을 분석할까? 바로 다음과 같이 **convolutional neural networks**을 사용한다.

<img src="/assets/images/spotify_convnet.PNG"><br>

그림을 보면 4개의 ConvNet과 3개의 Dense Layer을 이용한다. 입력은 Audio frame으며, ConvNet에서는 노래 시간 동안 학습된 feature을 이용하여 효과적인 통계를 내놓는다. 이런 과정이 끝나면 Nerual Network에서는 time signature, key, mode, temp, loudness을 포함하여 노래관한 내용을 도출한다.

즉, 노래를 분석하여 사용자가 들었던 노래 기록을 기반으로 사용자가 즐길만한 노래의 유사점을 찾아주는 것이다.

스포티파이에서는 추천 모델을 모두 연결하여 엄청 큰 시스템을 갖추고 있다. 여기서는 간략한 내용만 작성했으며 자세한 내용을 알고 싶다면 Reference 세션의 링크를 통하여 읽어보면 될 것 같다.


### Reference
[How Does Spotify Know You So Well?](https://medium.com/s/story/spotifys-discover-weekly-how-machine-learning-finds-your-new-music-19a41ab76efe)