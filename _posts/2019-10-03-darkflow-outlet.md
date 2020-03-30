---
layout: post
title: Darkflow | 두달간 사용하며 알게된 것들
tags: [Darkflow, TIP]
---

> 주관적인 글입니다. 이것이 학계의 정설은 아니며 피드백 환영합니다^ㅇ^  
> 아직 모델개발에 성공하지 못했기 때문에 과정중에 느낀 것들이고 틀릴 수 있습니다!
> 질문에 답변해주신분들(이 모님, 신 모님), 정보를 제공해준 블로그(운영자님)들 모두 고맙습니다🙏

<br>

## 0. 서론

인턴을 하며 딥러닝 공부를 시작했고, 모델 개발을 위해 darkflow를 이용해서 `데이터수집->전처리->학습->조금바꿔서학습->데이터수집->...` 의 과정을 반복했습니다.<br>  
<br>
소소하게 개인적으로 개발일지만 작성하고 있었는데, 한 분기가 끝났고 10월이 된 기념으로 지난 몇 주간 모델개발을 하며 느낀 점(_이라 쓰고 의식의 흐름이라고 읽는다._)을 기록하려고 합니다.  
<br>  
<br>

## 1. Label (class)

#### 자세를 나노단위로(방향따라) 나누는 것은 의미가 없다고 느낍니다.
(예. sleep_front, sleep_head, sleep_side)<br>  
<br>  
어차피 잠(sleep)이라는 것은 동일하기 때문입니다. 대신 데이터셋은 충분히 많아야 합니다.  (몇장..?)  Yolo 기본 모델을 돌려보면 '사람, 동물, 차'를 방향과 관계없이 똑같은 '사람, 동물, 차'로 인식하는 것을 알 수 있습니다. 따라서  **자는 강아지 데이터는 모두  `sleep`으로 라벨링 했습니다.**  
  <br>
  <br>

## 2. 전처리 순서
#### 1. 조건에 맞지 않는 사진 지우기 (솎아내기)
데이터의 질이 중요하다고 생각한다 -  **정확도&화질**<br>

#### 2. 데이터 이름 일괄적으로 변경하기 (예. a1, a2, a3, ...)
꼭 할 필요는 없지만, 정리할 때 더 편한 느낌<br>

#### 3. 라벨링 궈궈 
Yolo_mark(txt), labelImg(xml) 등 이용<br>

#### 4. annotation 형식에 맞추기 
([convert_txt_2_xml](https://github.com/ZZANZU/YOLO-convert-txt-2-xml))을 이용해서 txt로 라벨링한 것들만 xml로 바꿔주었다. (darkflow는 annotation으로 xml을 사용하기 때문) 

<br>
  <br>

## 3. 데이터 수집에 대하여

### 1) 데이터를 확보할 수 있는 창구 필요
데이터를 확보할 수 있는 방법을 많이 생각해 봐야할 것 같다. 크롤링, 지인찬스, 구매 등<br>

### 2) 데이터 수량이 정말 중요할까?
학습이 잘 되지 않자, 모든 것에 의심을 하기 시작했다. 알고보니 내 Labeling에 문제가 있는 건 아닐까?! 이것에 대한 정답은 모르겠지만 (여전히 학습이 잘 안되기 때문^^!) 확실한건 데이터의 양은  **다다익선**  
[![ㅠㅠ](https://user-images.githubusercontent.com/37680108/65928930-c96b4f80-e43a-11e9-95f3-c4bba0f1a1d3.PNG)](https://user-images.githubusercontent.com/37680108/65928930-c96b4f80-e43a-11e9-95f3-c4bba0f1a1d3.PNG)  
[원문링크](http://thesciencelife.com/archives/923)  
 <br>
<br>

## 4. 꿀팁

### 1) 문제가 생겼을 땐 Darkflow 깃헙의 Issue를 봐라.
특히 close된 것은 해결된 것! 따봉 많인 받은 답변을 우선적으로~!<br>

### 2) learning rate
초반엔 크게, 어느정도 내려오면 작게 설정하기. (시간단축)  <br>  
`--lr 0.0001` learning rate를 0.0001로 설정한다는 뜻.<br>  
cf) 1e-03 = 0.001<br>

### 3) Darkflow는 weights파일대신 meta파일 사용!
test할 때! 아마도 이게 학습된 모델<br>  
학습시 `--backup <폴더명>`에 지정한 폴더에 생긴답니다. (기본 : ckpt폴더)  <br>  
`--load -1` : 제일 최근 학습된 모델 불러오기.<br>  
`--load <체크포인트번호>` : <체크포인트>에 해당하는 모델 불러오기.<br>

### 4) Checkpoint 설정
\darkflow\darkflow\net\flow.py 70번째 줄에서 괄호부분을 바꿔준다. <br>   
ckpt = (i+1) % (self.FLAGS.save // self.FLAGS.batch) <- 이 괄호부분!  <br>  
`ckpt = (i+1) % 10`  step10마다 저장한다는 뜻<br>

### 5) threshold 설정
`--threshold 0.3`  30% 정확도로 박스를 친다는 뜻<br>

### 6) Tensorboard 이용하기
학습 커맨드에  `--summary /<저장경로>`  를 추가해준다.  <br>  
그러면 <저장경로>/train 안에 events가 저장이 된다.  <br>  
train폴더에서 cmd를 띄운 뒤

```
>tensorboard --logdir=./
```

를 입력해주고, 그 밑에 뜨는 링크로 접속하면 확인할 수 있다.<br>  

모든 TensorBoard Kill하려면
```
>pkill -f "tensorboard"
```

---
_! 예전 블로그에서 옮겨온 게시물입니다._