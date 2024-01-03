# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 전민규
- 리뷰어 : 이상원


# PRT(Peer Review Template)
- [X]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
    - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부
    - 피드백: 루브릭에서 제시한 대로 VGG16 모델 사용, 정확도 및 손실 곡선 제시, test accuracy 결과 출력
    - 모델 코드
        base_model.trainable = False
        import tensorflow as tf
        from tensorflow. keras.callbacks import EarlyStopping
        early_stopping = EarlyStopping(patience=10, restore_best_weights=True)
        #최종모델
        model = tf.keras.Sequential([
          base_model,
          global_average_layer,
          dense_layer,
          prediction_layer
        ])
    - 테스트 결과
        12/12 [==============================] - 1s 64ms/step - loss: 0.4452 - accuracy: 0.8556
        Test Loss: 0.44524750113487244
        Test Accuracy: 85.55858135223389%

- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
    - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다
    - 피드백 : 각 코드의 주석처리로 해당 코드의 기능 설명 실시
    - 코드 예시
    #이미지 데이터셋의 크기가 각각 다르기때문에 모델에 input시 크기를 맞춰줘야함으로 이미지resizing
    IMG_SIZE = 160 # 리사이징할 이미지의 크기
    #리사이징시킬 함수구현
    def format_example(image, label):
        image = tf.cast(image, tf.float32)  # image=float(image)같은 타입캐스팅의  텐서플로우 버전입니다.
        image = (image/255)  # 픽셀값의 scale 수정(정규화)
        image = tf.image.resize(image, (IMG_SIZE, IMG_SIZE))
        return image, label

- [X]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
    - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다
    - 피드백: 마지막 모델 활용 부분에서 경로 설정이 안되는 부분 에러로 기록함
    - 예시 코드
    ---------------------------------------------------------------------------
    FileNotFoundError                         Traceback (most recent call last)
    /tmp/ipykernel_47/4214472824.py in <module>
         16 flower_image_path ='/aiffel'
         17 filename = "flower_test.jpg"
    ---> 18 show_and_predict_image(flower_image_path, filename)
    
    /tmp/ipykernel_47/4214472824.py in show_and_predict_image(dirpath, filename, img_size)
          5 def show_and_predict_image(dirpath, filename, img_size=160):
          6     filepath = os.path.join(dirpath, filename)
    ----> 7     image = load_img(filepath, target_size=(img_size, img_size))
          8     plt.imshow(image)
          9     plt.axis('off')
    
    /opt/conda/lib/python3.9/site-packages/keras/preprocessing/image.py in load_img(path, grayscale, color_mode, target_size, interpolation)
        311       ValueError: if interpolation method is not supported.
        312   """
    --> 313   return image.load_img(path, grayscale=grayscale, color_mode=color_mode,
        314                         target_size=target_size, interpolation=interpolation)
        315 
    
    /opt/conda/lib/python3.9/site-packages/keras_preprocessing/image/utils.py in load_img(path, grayscale, color_mode, target_size, interpolation)
        111         raise ImportError('Could not import PIL.Image. '
        112                           'The use of `load_img` requires PIL.')
    --> 113     with open(path, 'rb') as f:
        114         img = pil_image.open(io.BytesIO(f.read()))
        115         if color_mode == 'grayscale':
    
    FileNotFoundError: [Errno 2] No such file or directory: '/aiffel/flower_test.jpg'
        
- [X]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
    - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
    - 피드백 : 전체 과정에 대한 회고 기록 남겨놓음. 과정에서 부족한 부분 등 자세히 넣음
        
- [X]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
    - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
    - 피드백: 각 단계마다 코드를 간결히 작성해서 문제 해결 실시
    - 코드 예시


# 참고 링크 및 코드 개선

```
# 사진을 캡쳐해서 넣고 싶었는데, 계속 붙여넣기가 안되, 어쩔 수 없이 직접 코드를 복사해서 붙여 놓았습니다.
# 과제 하느라 고생하셨습니다.

```
