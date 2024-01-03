added @ Wednesday Jan 3, 2024

## yolo.c

yolo.c
- train_yolo
    - 최종적으로 train_yolo를 보면, 외부에서 기록된 cfg file과 weight 파일을 가지고 네트워크를 구성하여 사용하는 꼴(parse_ network_cfg함수 사용)

- **test_yolo(char *cfgfile, char *weightfile, char *filename, float thresh)**
    - image.c
    - network.c
    - detection_layer
    - box.c

- run_yolo
    - user가 원하는 args를 체크하고 변수를 지정하는 역할만 수행
    - args값을 지정한 뒤에 이를 반영하여 test_yolo를 수행한다.


## image.c
color (r,g,b)의 경우를 (1,1,1) (0,0,0)을 제외하여 정해 놓음 
&rarr; colors[6][3]

get_color : 해당 픽셀의 색을 판단, ratio를 계산하고, 처음에 입력받은 c 를 가지고 얼마나 큰 값이(r) 나오는지 판단

get_pixel

detection_with_class : thresh 값과 비교해서 제일 높은 확률치를 업데이트 하는 형식
-> thres값과 비교 크면 업데이트? 작으면 업데이트 x &rarr; 최종적으로 제일 큰 prob 값 도출
이렇게 도출된 값과 클래스를 result_arr 에 저장하고 리턴

draw_detections_v3: detection_with_class에서 나온 결과들을 이용하여 detection을 수행하는 것 - boxing을 포함하여 수행한다.

detection det.는 결국 box에 구조체로 선언되어있음


## network.c
## detection_layer
- detection layer는 layer로 만든 구조체 -> 결국 layer와 모두 같으나 새로 정의만 해준 것
- make_detection_layer
    - 
- foward_detection_layer
    - location을  l.side로 인식하고,
    layer의 상태를 확인 ? softmax함수가 포함되어 있으면 수행
    network_state가 train 인지 test인지 확인 후 train 이면 수행

## box.c
convolutional_layer.c // network.c // layer.c // image.c // yolo_layer.c // network.c

1. activation_layer : 활성함수를 포함하는 레이어를 연결하는 역할  실제 활성함수는 activations.h에 포함되어 있음
2. activations.c

## layer
    layer의 struct를 정의하고 있고, 
    layer_type , activation 종류, cost_type 을 포함
 결국 l.@ 로 호출되는 layer는 cnn의 과정에 존재하는 모든 특징을 포함함
 &rarr;  layer로  cnn 의 단계를 구성하는 방식임

 4. convolutional_layer.c


 5. network.c
- make_network 의 함수는 결국 network를 사용할 메모리를 할당해주는 역할에 제한
    - 여러개의 layer로 network가 구성되므로 이를 가르키는 포인터 형태로 저장

- 

***
- xcalloc , xmalloc, xrealloc 의 역할
    - malloc, calloc, realloc의 역할을 수행하되, 메모리 할당 에러가 발생하는 경우 리턴을 제공하는 것
    - 동적으로 메모리를 할당하여 메모리 효율성을 높이는 것
    - 