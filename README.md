# machine_translation

한국정보화진흥원에서 운영하는 AI Hub에서 약 160만 개의 한국어-영어 번역(병렬) 말뭉치를 사용하여 기계번역을 실시하였다. 이 데이터셋을 사용하여 train데이터셋, test데이터셋, 그리고 validation데이터셋으로 분리한 후 step수를 변화시키기도 했고 train데이터셋을 대폭 늘려보기도 했다. 마지막으로 BPE의 유무에 따라서 성능이 어떻게 변할지 살펴보았다. 이를 통하여 step수는 최대 20만 번까지 높여보았는데 높아질수록 더욱 좋은 성능을 확인할 수 있었다. train데이터셋과 test데이터셋, validation데이터셋에 대해서는 각 96만, 32만, 32만 개로 나누었을 때, 즉 3:1:1의 비율로 나누었을 때 가장 좋은 성능을 보였다. BPE는 적용하고 시도했을 때와 제외하고 시도했을 때의 성능이 확실히 차이나는 것을 볼 수 있었다. 이러한 결과들을 통해 step수를 높게, 데이터셋은 3:1:1의 비율로, 그리고 BPE를 사용했을 때 제일 좋은 성능을 보임을 확인할 수 있었다.

1. 서론

 일상에서 쉽게 우리는 영어나 일어 혹은 다른 언어를 사용할 때, 당연하듯 네이버의 파파고나 구글의 번역기, 카카오의 i번역을 당연하듯이 키고 한글을 입력하여 우리가 원하는 뜻의 해당 언어로 된 문장을 얻는다. 이 주요 번역기들은 모두 기계 번역을 사용하는 좋은 예들이다. 이만큼 다른 나라 언어를 모르면서도 이 언어로 쉽게 사용하게 해주게 만들어 주고, 우리 생활에 가까이 있는게 기계 번역이다. 이미 이런 번역기들은 높은 수준의 성능을 가지고 있다. 
 하지만 영어로 표현하기 어려운 한글의 
관용어나 긴 문장에서는 성능이 높기 힘들다. 그 이유를 알고, 조금이라도 성능을 높이기 
위해 딥러닝을 사용한 기계번역을 통해 실험 조건을 바꾸어보며 어떻게 하면 성능이 높이는데 도움이 되고 최적화할 수 있는지 실험해 보았다. 먼저 좋은 번역을 위해 양질의 훈련 데이터들이 중요하다. 그 고품질의 데이터 약 160만개는 AI hub의 영한 구어체와 시사, 경제 등의 뉴스를 통해 수집할 수 있었다. 우리는 이 데이터를 토대로 병렬 코퍼스 필터링하여 학습데이터를 질 좋은 데이터로 만들었다.  준비한 데이터를 OpenNMT 2.0을 사용하여 데이터의 갯수, 반복 횟수, 전처리 단계에서의 데이터 bpe 여부에 따른 차이를 실험했다. 그리고 영어와는 다른 음 문자인 한글에서 영어로 
번역에서는 얼마나 성능의 차이가 있을지를 같이 실험해 보았다

2. Tokenization(BPE)

 기계번역을 하기에 앞서 가장 중요하다고 생각되는 단계는 바로 tokenization이다. 기계가 모르는 단어를 만나면 그 단어는 OOV(Out-Of-Vocabulary)라고 표현하는데, 모르는 단어를 만나 문제를 해결하지 못하는 상황인 OOV문제를 해결하기 위해서 tokenization이 필요하다. tokenization은 하나의 단어를 더 작은 단위의 의미있는 여러 서브워드들로 분리하는 일을 한다. 이러한 tokenization방법에는 여러 방법들이 있겠지만, 이 논문에서는 BPE이라는 방법을 사용하였다. 기본적으로 단어는 여러 subword들의 조합으로 이루어진다는 가정하에 적용되는 알고리즘인데, 실제로 영어나 한국어는 라틴어와 한자를 기반으로 형성된 언어이기때문에, 많은 단어들의 subword들로 구성되어있다. 따라서 적절한 subword들을 분리해주기만 한다면 OOV문제를 완화할뿐만 아니라 어휘수와 sparsitiy를 줄일 수 있다.


3. 실험조건
실험 조건은 다음과 같이 데이터의 개수와 반복 횟수(steps)를 달리하여 해보았다.

3.1 데이터 개수, 반복 횟수
![image](https://user-images.githubusercontent.com/79649052/124239897-225aa100-db55-11eb-8c87-22d47f13c710.png)
  

3.2 bpe 사용 여부
	Step	데이터	bpe 
영-한	20만	약 160만	O
영-한			X







3 한-영 번역
	Step	데이터	bpe 
![image](https://user-images.githubusercontent.com/79649052/124239954-32728080-db55-11eb-81e2-e4de2d2525bb.png)









4. 실험 결과


4.1 데이터 개수와 반복 횟수에 따른 실험 결과 (영-한)

![image](https://user-images.githubusercontent.com/79649052/124239984-3ef6d900-db55-11eb-98b6-14e75043c8ba.png)
	데이터	Step	BLEU	1-gram	2-gram	3-gram	4-gram

 기계 번역의 성능을 평가하기 위한 점수로 BLEU Score를 사용하였다. 뒤에 4가지 gram 점수는 순서쌍의 반복이다. 4.1의 실험결과를 통하여 데이터가 많을수록 더 좋은 BLEU 점수를 얻을 수 있는 것으로 확인된다.  그리고 반복 횟수도 달리하여 실험한 결과 20만번까지는 많으면 많을수록 좋아졌다.


4.2 BPE 사용 여부에 따른 실험 결과(영-한)

![image](https://user-images.githubusercontent.com/79649052/124240031-4ae29b00-db55-11eb-9637-e3424daf242b.png)
 BPE를 사용했을 때, 토큰화로 인해 보다 높은 BLEU를 얻을 수 있었다.


5. 결론
본 논문은 step 수, 데이터셋의 비율, BPE 여부에 대한 성능 평가를 진행하였다. 데이터는 한국정보화진흥원의 AI Hub에서 약 160만 개의 한국어-영어 번역(병렬) 말뭉치를 사용하였다. 그 결과로 step 수는 높을수록 더 좋은 성능을 보였고 데이터셋은 3:1:1 비율로 나눈 것이 가장 좋은 성능을 보였고 BPE는 있고 없고의 차이가 극심함을 볼 수 있었다. 이를 바탕으로 BPE 즉 tokenization 방법을 더 개선하면 성능이 많이 높아진다. 또 컴퓨터의 성능이 더 좋았다면 step 수를 훨씬 더 높게 잡고 실행을 해볼 수도 있었을 것이다. 그리고 한-영에서의 성능이 월등히 높게 나왔는데 이 부분은 한글에서의 같은 의미의 단어가 여러가지로 표현되는 것이 영어로 번역했을때 다대일 대응으로 되어 훨씬 성능이 높게 나왔다고 추측한다. 추후에 이 부분에 대해 더 명확한 결과를 얻고자 실험해보려 한다.


6. 참고문헌

[1] 박찬준, 김경민, 임희석, “병렬 코퍼스 필터링과 한국어에 최적화된 서브 워드 분절 기법을 이용한 기계번역”
[2] 박찬준, 임희석, “공공 한영 병렬 말뭉치를 이용한 기계번역 성능 향상 연구”

