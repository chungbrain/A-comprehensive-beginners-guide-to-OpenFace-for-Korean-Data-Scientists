# OpenFace-Easy to Understand in Korean beginner

OpenFace 는 open source 기반 얼굴표정 움직임 분석 툴킷 이다. 
본 논문을 한글로 정리하는 이유는 한국사람들에게 OpenFace Project를 쉽게 빠르게 이해하고 알리기 위함이다. 이번 정리는 단순히 논문을 전달하는 일뿐만 아니라 필자의 의견을 comment로 남기려고 한다.
간단하게 논문을 소개하자면 저자는 Tadas이고 2018년 현재 CMU 에서 post-doc를 하고 있다. OpenFace는 Cambridge 대학에서 진행한 논문으로 보인다. 본 논문은 2016년 WACV Conference paper로 발표되었고 2018년 05월 현재까지 222회가 넘게 인용되었다. (IEEE Winter Conference on Applications of Computer Vision, Lace Placid, NY, March 2016) 비록 conference paper이지만 이 분야에서 높은 영향력이 있는 발표라고 말할 수 있겠다.
사실 2016년에 OpenFace 논문이 발표되기 이전에 Tadas는 2013~2015년동안 3개의 논문이 발표 되었다. 3개의 논문은 OpenFace 의 중요 요소들(Facial landmark detection and tracking, Eye gaze tracking, Facial Action Unit detection)로써 기반을 다진다. 
 
그럼 하나씩 논문을 살펴보자

** Abstract
지난 수년 동안 얼굴표정행동(facial behavior)을 자동으로 분석하고 이해하는 것에 대한 관심이 증가하고 있다. Openface는 computer vision과 machine learning 관련 연구자들, affective computing 커뮤니티, 그리고 interactive 응용프로그램에 관심이 있는 사람들을 위한 툴이다. 
 OpenFace는 Facial landmark detection, head pose estimation, facial action unit recognition, eye gaze estimation 이 모두 가능한 첫 번째 open source다. Computer vision 알고리즘은 OpenFace의 core이고 최상의 결과를 나타낸다. 게다가, OpenFace는 real-time 으로 처리 할 수 있도록 구성되었고, 그 예로 범용 webcam으로부터 데이터를 real-time으로 처리하고 위에서 소개된 각 방법들이 적용되는것을 demo로 보여준다.  마지막으로 OpenFace는 lightweight messaging system 을 통해서 다른 응용프로그램/장비와 쉽게 integration된다. 

* Comment 
필자는 OpenFace를 지난 3개월간 모르고 있었다. 지난 세월 나는 OpenCV 기반으로 위와 같은 방법들을 내 관심분야(error resilient health정보 예측)에 적용해왔다. 하지만, 내 관심분야의 결과물은 만족스럽지 못했다. 왜냐하면, real-time으로 처리되어야 하는 video image processing 에서 매 frame 마다 추출되는 특징들이 매 frame 마다 varying 하기 때문이다. 곧, Frame-varying한 preprocessing결과물은 내 프로젝트의 결과에 나쁜 영향을 미치고 있었다. OpenFace를 알게된 후 내 악몽은 사라졌으며 스트레스성 폭식으로 인한 체중증가는 멈추게 되었다. OpenFace를 적용전/후의 필자의 결과물은 본 논문 후반부에 같이 소개하도록 하겠다.
그럼 역사 공부를 위해서 introduction으로 옮겨 가보자.

**1. introduction
서론 첫 문장에 인상 깊은 표현이 나타났다. 저자는 Facial expression과 facial behavior를 구분했다. Facial expression은 웃는 표정, 슬픈 표정, 화난 표정 등 typical한 정적인 표현에 대한 단어라면, facial behavior은 아마도 동적이고 반복적인 행동으로 생각되는데, 예를 들어서 긴장하면 습관적으로 코를 벌렁 이던 지, 입맛을 다신다던 지, 입술을 깨문다던 지 같은 행위를 나타내는 것 생각한다. 다시 논문으로 돌아와서, 얼굴은 비언어 소통에서 매우 중요한 채널이다. 얼굴표정행동 분석은 HCI를 용이하게 하는 다양한 응용프로그램에 사용되었다. 더욱 최근에는 의학 분야에서 (우울증, 외상 후 장애 등), 산업, 교육, 예술 등 자동얼굴표정 분석시스템에서 개발 이용되고 있다. 
본 논문에서 facial behavior를 facial landmark motion, head pose (orientation and motion), facial expression, eye gaze 로 정의한다. 각 4개의 modalities 다양한 연구들에서 사용되었다. 때때로는 하나씩 연구에 employ되었고, 연구의 용도에 따라서는 위 4개중에 2~3개씩 묶여 사용되는 경우도 있기 때문에 모두가 face기반 emotion 관련 연구에 중요한 role을 가진다. 다른 연구자들에 의한 논문 중에 예를 들어서 설명한 바로는 (1: 첫 번째 예시로써)automatic detection과 facial action unit분석이 emotion recognition system에서 매우 중요한데 action unit들은 (얼굴의 각 부위별 움직임을 수치화시킨 것으로 추정됨)의 emotion을 추정한 때 action unit들의 presence와 intensity 를 수치화된 값 기반으로 활용했다. (2: 두 번째 예시로써) action unit들과 head pose, 그리고 gesture를 동시에 활용해서 활용한 연구들이 있었다. (물론 emotion과 social signal perception 간의 관계를 연구한 연구였음) (3: 세 번째 마지막 예시로써) gaze detection은 집중력 평가, 사교성 평가?, 정신건강 척도? 그리고, emotion의 세기까지 평가하는 방법으로 활용되었다. 
아무튼 위와 같이 많은 face정보를 기반으로 emotion분석 관련 학술적인 접근이 많았지만, 2016년 전까지는 제대로 된 open source 하나 없었다는 주장이다. 여기서 주목할 점은 open source는 몇 개 있었지만 제대로 된 것이 없었다는 문장에 주목하자. 저자는 최신에 release되고 있는 “고급기법기반 알고리즘”과 “freely 사용할 수 있는 toolkit”간에 서로 Gap이 존재했다고 언급했다. (심지어 저자는 real-time & interactive 환경으로 toolkit을 만들어야 한다고 주장한다)
다른 연구자들에 의해서 발표된 지난 연구들이 어떤 한계점을 갖고 있었다고 말하고 있는데 이 부분은 한계점을 정리된 단어로만 나열하고 넘어가겠다. (따라 하기 불편함- 개발환경차이/상세한 설명부족/부분적인 소스코드 오픈) 
* comment
저자는 아마도 다음과 같은 말을 하고 싶은 것 같다. “내가 한번 정리해볼게! 난 너희들이 이 분야를 뛰어들 때 보다 쉽고 편리하게 접근했으면 좋겠어! 내가 최신식으로 준비했거든!” 
 
**2.Previous work
우선 본 논문은 facial landmark detection, head pose, eye gaze, action unit estimation 에 대한 상세한 full review는 하지 않는다. 대신 이번 논문에서는 각 방법에 대한 지난 연구들의 성취를 다룬다. 
*facial landmark detection
*Head pose estimation
*AU recognition
*Gaze estimation

**3.OpenFace pipeline
자 이제부터 저자 Tadas가 말하는 논문(결과)의 uniqueness를 (차별 점) 살펴보자. 우선 본 논문은 위 4개의 중요 요소들(facial landmark detection, head pose, eye gaze, action unit estimation) 의 방법론을 상세하게 풀이했다. 어떤 단계를 거쳐서 진행되었는지, 어떤 얼굴의 특징들이 개발에 사용되었는지, 그리고 기존의 모델을 이용해서 person calibration을 어떤 방식으로 (facial action unit의 강도와 존재유무를 판별할 때) 적용했는지 설명하고 있다. 

*3.1 Facial landmark detection and tracking
OpenFace는 Facial landmark detection과 tracking을 위해서 Conditional Local Neural Fields (CLNF)를 제안하고 있다. CLNF는 Constrained Local Model(CLM: 은 advanced Patch expert와 optimization function로 구성)의 instance다. Point distribution model 은 landmark shape variation을 capture한다. Patch experts 가 local appearance variation을 capture하는 역할을 한다. 
*comment
사실 OpenCV기반으로 face detection (viola johns 방법)을 할 때 매 frame마다 얼굴을 추출하는 위치에 variation이 있기 때문에 segmented face image에 대한 위치의 변화가 불가피 했다. 필자는 segmented image로부터 얻는 face color estimation 값이 변화되었다. Kalman filter를 적용해서 error를 피할 수 없었다.

*3.1.1 model novelties
2d image에서 feature를 추출하기 위해서 과거 전통적인 image processing method는 단순한 spatial filter 기반의 접근방법을 사용해왔다. 좀 더 구체적으로 들어가자면, video frame의 object tracking을 연속적으로 하기 위해서는 particle filter가 널리 사용되고 있다. Particle filter는 Monte Carlo 방법을 기반으로 대표 값을 선정하는데, 이는 non-Gaussian/nonlinear 상태를 추정하는데 효과적이다. Particle filter 내에서 state transition 모델과 appearance 모델은 중요한 확률 모델로 역할을 맡고 있다. State transition 모델은 current target상태를 previous target상태를 기반으로 예측하기 위해 사용되었다.(state transition모델은 Gaussian distribution 모델과 constant velocity모델을 기반으로 한다.) 그 중 Gaussian distribution모델기반 방법은 frame 간의 움직임이 짧게 랜덤으로 발생할 때 좋은 성능을 나타내고 계산의 효율성과 간결성 때문에 널리 사용되고 있다. 하지만, target의 움직임이 빠르거나 변화의 폭폭 클 때는 more particle들이 계산에 동원되기 때문에 계산속도에 영향(복잡-느려짐)을 미친다. 한편, Constant velocity모델은 Gaussian distribution 모델과 상반된 수학적 개념을 적용한 방법이다. velocity모델의 단점은 current frame에서 velocity가 현저하게 작아지면 (previous frame 와 비교했을 때) 잘못된 variance때문에 target을 추정을 부정확하게 할 수 있다. 
State transition모델 이외에 appearance 모델도 다양한 방법들이 존재한다. (generative model, discriminative model, and collaboration model) generative model은 background는 무시하고 foreground 정보에 집중하는 방법이고, discriminative model은 target tracking process를 classification process로 접근하는 방법이며, collaboration model은 두 개의 장점을 취한 방법이다. 
위와 같은 model기반의 접근 방법이 최근 범용적으로 관련연구에 적용되고 있다고 언급하고 있다. 하지만 기존 방법들은 complex scenes에 대해서 object tracking 성능이 떨어지는 문제점을 보인다. Lijun He et al 2018 논문에 의하면 이와 같은 방법들은 tracking drift or failure를 유발한다고 언급했다. 
이번 문단에서 저자는 CLNF 모델 기반의 방법으로 68개의 facial landmarks를 검출했다고 했다. 더 나아가 PDM과 patch expert를 함께 사용해서 여러 landmark들을 fitting했다고 한다. 이를 위해서는 face validation step이 필수적이고 (face align을 piecewise affine warp를 통해서 진행함), 이것은 LFPW and Helen training set기반의 간단한 CNN기반으로 구현했다. Validation step이 fail하면 model을 reset해야한다는 단점은 존재한다. 


*3.1.2 implementation detail
OpenFace내의 PDM은 LFPW and Helen training set기반으로 구축되었다. 이 모델을 통과한 결과물로써 34개의 non-rigid parameters와 6개의 rigid shape을 산출한다. 한편, CLNF patch expert를 training하기 위해서 Multi-PIE, LFPW, Helen training set을 사용했는데 7 views와 4가지의 scales로 독립적으로 training했기 때문에 총 28개의 set을 산출했다. (논문만 읽고 필자도 저자의 의도를 명확하게 파악하기 힘들다. Source code를 집적 보고 다시 기술하도록 하겠다.)

CLNF 모델의 initial face detector는 dlib library를 이용했고, 여기서 68개의 facial landmark를 feature로 얻을 수 있다. 필자는 얻어진 초기 features를 기반으로 최적화를 적용했다고 예측하고 있다. OpenFace는 multiple faces에 대해서도 tracking가능하다. 

**3.2 Head pose estimation
본 논문의 model은 head pose (orientation, translation)를 추출 할 수 있다. CLNF 는 내부적으로 68개의 landmarks에 대한 3D shape을 갖고 있기 때문에 orthographic camera projection을 통해서 Head pose를 추정한다. (PnP: perspective-n-points problem 를 활용했다. 3D points 와 2D projection간 matching방법, 자세한 사항은 http://icwww.epfl.ch/~lepetit/papers/lepetit_ijcv08.pdf 이 논문을 참고하면 좋을 것 같다)

**3.3 Eye gaze estimation
눈의 움직임은 video image 처리 분야에서 deformable shape을 다뤄야 하기 때문에 결과의 정확성과 알고리즘의 효율성 동시에 만족시키는 어려운 일이라 생각한다. 본 논문에서는 CNLF를  eye region landmark (눈커플, 홍채, 동공)를 검출하는데 사용했다. Training set으로 SynthesEyes training dataset을 사용했다. 각 eye별로 눈과 동공의 위치를 검출해서 gaze를 추정했다. 

**3.4 Action Unit detection
Action unit은 사람의 표정을 각 feature별로 논리화(presence) 및 수치화된(intensity) 값으로 변환하는 것이다. 
*3.4.1 model novelties
Action unit을 위해서 SEMAINE and BP4D 데이터셋을 이용했고 trained SVM model을 활용했다. 
*3.4.2 implementation details
Facial appearance feature를 추출하기 위해서 본 논문에서는 similarity transform을 사용했다. 결과물로써 112 X 112 pixel 의 얼굴 이미지을 산출했고 동공간의 거리는 45pixel로 맞췄다. 이후 HOG (histogram of oriented gradients) 를 aligned image 에 적용하여 feature를 추출했다. 이를 통해서 4464 dimensional vector가 face를 describe하기 위해서 추출된다. Dimension을 줄이기 위해서 PCA를 사용한다. 그리고 facial expression dataset으로는 CK+, DISFA, AVEC2011, FERA 2011, FERA 2015 가 사용되었다. PCA후에 1391개의 축소된 dimension이 산출된다.  


***4. Experimental evaluation
이번 section은 OpenFace의 성능을 정량적으로 평가한 결과를 나타내고 있다. 각 sub system 마다 결과가 나타나 있고 그 당시 발표된 다른 연구들과 성능을 비교한 결과를 보이고 있다. 
**4.1 Landmark detection
**4.2 Head pose estimation
**4.3 Eye gaze estimation
**4.4 Action Unit recognition
뭐 잘된다는 얘기다. 넘어가겠다. 

***5. Interface
OpenFace를 활용하는 3가지 방법이 있다. (1) GUI 로 활용하는 방법 (2) command line으로 활용하는 방법 (3) real-time messaging system (ZeroMQ를 활용하는 방법). 어떤 방식으로도 OpenFace를 cross-platform환경에서 이용 할 수 있다. Facial landmark, shape parameter, action unit, and gaze vector를 csv file로 export할수 있다. HOG feature는 matlab으로 읽을수 있는 binary file로 저장된다. 게다가, OpenFace는 emotion prediction, medical condition분석, social signal분석에도 활용된다. 본 논문에서는 ZeroMQ기반의 실시간 처리가능하다는 점을 다시한번 힘주고 있다.  


***6. Conclusion
본 논문은 처음으로 fully 공개된 Open source 기반 real-time facial behavior 분석 시스템이다.
OpenFace는 computer vision, machine learning, and Affective computing등에 활용될 수 있는 유용한 tool이다. 
*comment 
OpenFace는 좋은 툴이다 저자의 의견에 공감한다. 노고에 박수를 보내고 Tadas에게 감사의 마음을 전하고 싶다. 끝!
