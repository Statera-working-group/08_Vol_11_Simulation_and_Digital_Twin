**Volume 11. Simulation and Digital Twin**

# Chapter 04. Sensor Simulation

## 04.01. Sensor Simulation Fidelity and Noise Modeling

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 시뮬레이션(Sensor Simulation)은 로봇이 시뮬레이션 환경(Simulated Environment)과 상호작용할 때 실제 물리 센서(Physical Sensor)가 생성할 측정값을 재현하는 과정이다. 그 목적은 단순히 이상적인 기하학적 관측값(Ideal Geometric Observation)을 제공하는 것이 아니라 실제 센서가 가지는 불확실성(Uncertainty), 한계(Limitation), 시간적 동작 특성(Timing Behavior), 고장 특성(Failure Characteristics)을 근사하는 데 있다. 따라서 시뮬레이션 및 디지털 트윈(Digital Twin) 워크플로에서 센서 충실도(Sensor Fidelity)는 인지(Perception), 위치 추정(Localization), 매핑(Mapping), 제어(Control), 학습(Learning) 알고리즘이 가상 세계를 얼마나 현실적으로 경험하는지를 결정한다.

센서 충실도(Sensor Fidelity)는 시뮬레이션된 측정값이 실제 물리 센서의 동작을 얼마나 가깝게 재현하는지를 나타낸다. 저충실도 센서(Low-Fidelity Sensor)는 시뮬레이터에서 정확한 거리, 자세(Pose), 픽셀값 등을 직접 반환할 수 있지만, 고충실도 모델(Higher-Fidelity Model)은 측정 불확실성(Measurement Uncertainty), 유한 해상도(Finite Resolution), 지연시간(Latency), 환경 간섭(Environmental Interference), 보정 오차(Calibration Error), 하드웨어 한계(Hardware Limitation)까지 포함한다. 필요한 충실도는 무조건 최대화하기보다 공학적 목적에 따라 선택해야 한다.

이상적인 센서 모델(Ideal Sensor Model)은 실제 시뮬레이션 상태(True Simulated State)를 정확한 관측값으로 변환하는 과정으로 개념화할 수 있다. 그러나 실제 센서는 여러 종류의 교란(Disturbance)에 영향을 받으므로 실용적인 모델은 측정값 = 이상적 응답(Ideal Response) + 체계적 오차(Systematic Error) + 확률적 오차(Stochastic Error) + 시간적 오차(Temporal Error)로 표현할 수 있다. 이러한 분해 방식은 각각의 오차 원인을 독립적으로 모델링하고, 보정하고, 무작위화하며, 검증한 후 완전한 센서 시뮬레이션으로 통합할 수 있다는 장점이 있다.

랜덤 노이즈(Random Noise)는 예측하기 어려운 측정값의 변동을 의미하며 일반적으로 확률분포(Probability Distribution)를 이용하여 근사한다. 영평균 가우시안 노이즈(Zero-Mean Gaussian Noise)는 여러 개의 작은 독립적인 교란이 결합될 경우 정규분포에 가까운 특성을 나타내기 때문에 기본적인 모델로 자주 사용된다. 그러나 실제 센서는 비가우시안 분포(Non-Gaussian Distribution), 비대칭 오차(Asymmetric Error), 이상치(Outlier), 양자화 효과(Quantization Effect), 거리 의존적 불확실성(Range-Dependent Uncertainty)을 나타낼 수 있다. 따라서 실제 하드웨어 데이터가 존재한다면 측정된 센서 특성을 기반으로 노이즈 분포를 결정하는 것이 바람직하다.

체계적 오차(Systematic Error)는 지속적이거나 예측 가능한 편차를 발생시킨다는 점에서 랜덤 노이즈와 다르다. 대표적인 사례로 카메라 내부 파라미터 보정 오차(Camera Intrinsic Calibration Error), 라이다 거리 오프셋(LiDAR Range Offset), 관성 측정 장치 편향(IMU Bias), 위성항법시스템 위치 편향(GNSS Position Bias), 장착 정렬 오차(Mounting Misalignment), 스케일 팩터 오차(Scale-Factor Error)가 있다. 랜덤 노이즈만 포함하는 시뮬레이션은 이러한 영향을 과소평가하여 알고리즘이 실제보다 지나치게 강건한 것처럼 보이게 할 수 있다. 따라서 보정(Calibration), 위치 추정(Localization), 센서 융합(Sensor Fusion), 장시간 자율운행(Long-Duration Autonomous Operation)을 평가할 때 체계적 오차 모델링이 특히 중요하다.

편향(Bias)과 드리프트(Drift)는 시간이 지남에 따라 오차가 누적되는 센서에서 특히 중요하다. 예를 들어 관성 측정 장치(IMU)는 거의 일정한 편향과 함께 천천히 변화하는 편향 불안정성(Bias Instability), 그리고 빠르게 변화하는 측정 노이즈(Measurement Noise)를 포함할 수 있다. 가속도와 각속도를 적분하여 속도, 자세, 위치를 추정하면 작은 오차도 상당한 상태 추정 편차(State Estimation Deviation)를 발생시킬 수 있다. 따라서 센서 시뮬레이션에서는 모든 샘플을 통계적으로 독립적인 값으로 처리하기보다 순간적인 불확실성과 시간적 상관관계(Temporal Correlation)를 함께 재현해야 한다.

해상도(Resolution)와 양자화(Quantization)는 센서 현실성을 높이는 또 다른 요소이다. 실제 센서는 아날로그-디지털 변환기(Analog-to-Digital Converter), 이미지 픽셀(Image Pixel), 엔코더 카운트(Encoder Count), 거리 빈(Range Bin) 등의 측정 메커니즘으로 인해 임의의 연속적인 값을 무한한 정밀도로 표현할 수 없다. 양자화는 연속적인 시뮬레이션 값을 유한한 측정 단위로 변환하는 방식으로 모델링할 수 있다. 이러한 오차는 작게 보일 수 있지만 정밀 제어(Precision Control), 저속 운동 추정(Low-Speed Motion Estimation), 근거리 센싱(Short-Range Sensing), 미세한 측정값 차이에 의존하는 알고리즘에서는 중요한 영향을 줄 수 있다.

실제 센서는 또한 제한된 측정 범위(Measurement Range)와 시야각(Field of View)을 가진다. 시뮬레이션 센서는 최소 및 최대 감지 거리, 각도 범위(Angular Coverage), 사각 영역(Blind Region), 포화(Saturation), 클리핑(Clipping), 유효하지 않은 측정값(Invalid Measurement)을 재현해야 한다. 카메라 영상 밖의 객체, 라이다 측정 범위를 벗어난 객체, 레이더 감도 이하의 객체, 센서의 최소 감지 거리 안에 위치한 객체가 비현실적으로 완벽한 관측값을 생성해서는 안 된다. 이러한 물리적 제약은 랜덤 측정 노이즈만큼이나 인지 시스템의 구조에 큰 영향을 미친다.

환경 조건(Environmental Condition)은 센서 품질을 동적으로 변화시킬 수 있다. 카메라 측정은 조명(Illumination), 노출(Exposure), 반사(Reflection), 모션 블러(Motion Blur), 날씨(Weather), 광학적 특성(Optical Property)에 영향을 받는다. 라이다(LiDAR)는 표면 반사율(Surface Reflectivity), 입사각(Incidence Angle), 대기 입자(Atmospheric Particle), 다중경로 효과(Multipath Effect)의 영향을 받을 수 있다. 레이더(Radar) 응답은 형상, 재질 특성, 도플러 특성(Doppler Behavior), 레이더 단면적(Radar Cross Section)에 의존하며, 위성항법시스템(GNSS)은 신호 차단과 다중경로의 영향을 받을 수 있다. 따라서 고충실도 시뮬레이션은 모든 환경에서 고정된 노이즈를 적용하기보다 센서 오차를 환경 상태와 연결해야 한다.

시간적 충실도(Temporal Fidelity) 역시 중요하다. 실제 로봇 센서는 측정값을 순간적으로 전달하지 않으며 각각 고유한 샘플링 주파수(Sampling Frequency)로 동작하고 노출 시간(Exposure Time), 스캔 시간(Scanning Duration), 처리 지연(Processing Delay), 통신 지연(Communication Latency), 타임스탬프 불확실성(Timestamp Uncertainty), 지터(Jitter)를 발생시킬 수 있다. 여러 센서 역시 비동기적으로 동작한다. 완벽하게 동기화된 관측값을 생성하는 시뮬레이션은 센서 융합과 상태 추정에서 발생하는 중요한 문제를 숨길 수 있으므로 타임스탬프와 동기화(Synchronization) 특성은 현실적인 센서 모델링의 핵심 요소이다.

센서 지연시간(Sensor Latency)은 시뮬레이션에서 표현되는 물리적 사건이 발생한 시점과 해당 측정값이 소프트웨어에 전달되는 시점 사이의 지연으로 모델링할 수 있다. 이동 중인 로봇에서는 수십 밀리초 정도의 지연만으로도 카메라, 라이다, 관성 측정 장치(IMU), 레이더, 오도메트리(Odometry) 데이터 사이에 공간적인 불일치가 발생할 수 있다. 보다 정교한 시뮬레이션에서는 센싱 지연(Sensing Delay), 내부 처리 지연(Internal Processing Delay), 네트워크 전송 지연(Network Transmission Delay), 소프트웨어 스케줄링 지연(Software Scheduling Delay)을 구분하여 시간 오차를 체계적으로 분석할 수 있다.

고장 모델링(Failure Modeling)은 센서 시뮬레이션을 정상적인 동작 상태의 노이즈 모델링 이상으로 확장한다. 실제 센서는 일시적으로 측정값을 잃거나, 유효하지 않은 값을 반환하거나, 패킷 손실(Packet Loss), 포화, 부분적인 가림(Partial Occlusion), 환경 조건에 따른 성능 저하를 경험할 수 있다. 제어된 드롭아웃(Dropout)과 고장 이벤트(Fault Event)를 적용하면 인지 및 자율주행 소프트웨어가 센싱 성능 저하를 감지하고 대체 정보원, 제한된 기능(Reduced Functionality), 고장 관리 상태(Fault-Management State)로 안전하게 전환할 수 있는지를 검증할 수 있다.

여러 센서가 로봇에 장착되는 경우 공간적 보정 오차(Spatial Calibration Error)도 표현해야 한다. 각 센서는 로봇 좌표계(Robot Coordinate System)에 대한 위치와 방향을 정의하는 외부 파라미터 변환(Extrinsic Transformation)을 가진다. 작은 장착 편차도 특히 원거리에서 센서 모달리티(Sensor Modality) 사이의 체계적인 불일치를 발생시킬 수 있다. 이러한 변환값에 의도적인 변동을 적용하면 보정 알고리즘을 평가할 수 있으며, 기계적 공차(Mechanical Tolerance), 진동(Vibration), 유지보수(Maintenance), 센서 교체(Sensor Replacement)에 대해 인지 파이프라인이 얼마나 민감한지를 확인할 수 있다.

실용적인 충실도 전략(Fidelity Strategy)은 일반적으로 여러 단계의 시뮬레이션 수준을 사용한다. 초기 소프트웨어 개발에서는 계산 비용이 낮고 결정론적(Deterministic)인 단순화된 센서를 사용하여 빠른 디버깅과 대규모 테스트를 수행할 수 있다. 이후 검증 단계에서는 보정된 노이즈, 지연시간, 환경 영향, 현실적인 렌더링(Realistic Rendering)을 추가할 수 있다. 고충실도 레이 트레이싱(High-Fidelity Ray Tracing)이나 정교한 물리 기반 센서 모델(Physics-Based Sensor Model)은 광학적, 기하학적, 전자기적 현실성이 실제 평가 대상 알고리즘에 중요한 영향을 미치는 경우에 선택적으로 사용하는 것이 효율적이다.

이러한 단계적 접근 방식이 중요한 이유는 센서 현실성(Sensor Realism)에 계산 비용이 수반되기 때문이다. 사실적 카메라 렌더링(Photorealistic Camera Rendering), 고밀도 레이 캐스팅 라이다(Dense Ray-Cast LiDAR), 물리 기반 레이더(Physics-Based Radar), 복잡한 환경 상호작용(Environmental Interaction)은 상당한 그래픽처리장치(GPU) 자원을 소비할 수 있다. 지나치게 높은 충실도는 특정 실험의 유효성을 개선하지 못하면서 시뮬레이션 처리량을 감소시킬 수 있다. 반대로 충실도가 지나치게 낮으면 시뮬레이션과 현실 사이의 격차(Sim2Real Gap)가 커질 수 있다. 따라서 효과적인 시뮬레이션은 계산 효율성과 목표 시스템에 중요한 불확실성 메커니즘 사이의 균형을 유지해야 한다.

노이즈 파라미터(Noise Parameter)는 임의로 선택하기보다 실제 물리 측정을 통해 도출하는 것이 바람직하다. 실제 센서를 통제된 조건에서 동작시키고 출력을 신뢰할 수 있는 기준값(Reference)과 비교할 수 있다. 통계적 분석을 통해 평균 오차(Mean Error), 분산(Variance), 편향 안정성(Bias Stability), 거리 의존성(Range Dependency), 드롭아웃 확률(Dropout Probability), 시간적 상관관계를 추정할 수 있다. 이렇게 측정된 특성을 시뮬레이터의 파라미터로 적용하면 현실에서 시뮬레이션으로의 보정(Real-to-Simulation Calibration) 과정을 구축하여 가상 실험을 실제 하드웨어의 특성에 더욱 가깝게 만들 수 있다.

검증(Validation)은 동일하거나 동등한 조건에서 시뮬레이션 센서 출력과 실제 센서 출력을 비교하는 과정이 필요하다. 주요 비교 대상에는 오차 분포(Error Distribution), 공간 해상도(Spatial Resolution), 감지 확률(Detection Probability), 시간적 동작(Temporal Behavior), 주파수 특성(Frequency Characteristics), 후속 알고리즘 성능(Downstream Algorithm Performance)이 포함된다. 표준편차(Standard Deviation)와 같은 하나의 통계값만 일치시키는 것으로는 충분하지 않다. 평균적인 오차가 동일하더라도 비현실적인 시간적 또는 환경적 특성을 생성한다면 인지 알고리즘이 실제 환경으로 일반화되지 못할 수 있다.

머신러닝(Machine Learning)과 피지컬 AI(Physical AI)에서는 완벽하게 보정된 시뮬레이션이 항상 최종 목표가 되는 것은 아니다. 도메인 랜덤화(Domain Randomization)는 시뮬레이션 에피소드마다 노이즈 크기, 보정값, 조명, 센서 위치, 지연시간 및 기타 파라미터를 의도적으로 변화시킨다. 이를 통해 모델은 하나의 이상적인 가상 센서 구성만 학습하는 대신 다양한 센싱 조건의 분포를 경험하게 된다. 이러한 접근은 시뮬레이션에만 존재하는 특정 규칙성에 대한 의존도를 낮추고 실제 환경의 불확실성에 대비하도록 하여 강건성(Robustness)을 향상시킬 수 있다.

센서 시뮬레이션은 궁극적으로 독립적인 렌더링 기능이 아니라 전체 로봇 시스템 모델(Robotic System Model)의 일부로 다루어야 한다. 카메라, 라이다, 레이더, 관성 측정 장치(IMU), 힘 센서(Force Sensor), 촉각 센서(Tactile Sensor), 위성항법시스템(GNSS)은 동일하게 변화하는 물리적 상태에 대해 서로 다른 관측값을 제공하며, 각각의 불확실성은 인지 및 센서 융합 알고리즘을 통해 상호작용한다. 따라서 전체 시뮬레이션 아키텍처는 센서 충실도를 물리 시뮬레이션(Physics Simulation), 환경 모델링(Environment Modeling), 동기화, 합성 데이터 생성(Synthetic Data Generation), 시뮬레이션-현실 전이 검증(Sim2Real Validation)과 연결해야 한다.

성숙한 센서 시뮬레이션 워크플로(Sensor-Simulation Workflow)는 이상적인 센싱(Ideal Sensing), 보정된 오차 모델(Calibrated Error Model), 환경 영향(Environmental Effect), 시간적 동작, 고장 주입(Failure Injection), 도메인 랜덤화를 단계적으로 연결한다. 목표는 실제 하드웨어의 모든 미세한 특성을 완벽하게 복제하는 것이 아니라 로봇의 의사결정에 영향을 미치는 핵심 특성을 충실하게 재현하는 것이다. 이러한 원칙에 따라 충실도를 선택하고 검증하면 시뮬레이션은 인지 시스템 개발, 자율성 평가(Autonomy Evaluation), 학습 데이터 생성(Training Data Generation), 실제 로봇 배치 이전의 위험 감소(Risk Reduction)를 위한 실용적인 공학 도구가 된다.

## 04.02. Camera Sensor Simulation RGB Depth Fisheye [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

카메라 센서 시뮬레이션(Camera Sensor Simulation)은 로봇 비전 시스템(Robotic Vision System)이 가상 환경을 RGB, 깊이(Depth), 어안(Fisheye) 영상 모델을 통해 관측하는 방식을 재현한다. 시뮬레이터는 완벽한 장면 기하정보(Scene Geometry)를 인지 소프트웨어에 직접 제공하는 대신, 3차원 장면을 광학 기하(Optical Geometry), 해상도(Resolution), 시야각(Field of View), 노출(Exposure), 깊이 특성(Depth Characteristics), 왜곡(Distortion), 노이즈(Noise)의 영향을 받는 카메라 측정값으로 변환한다. 이를 통해 비전 알고리즘은 실제 물리 카메라와 유사한 인터페이스를 기반으로 동작할 수 있다.

RGB 카메라 모델(RGB Camera Model)은 3차원 점을 월드 좌표계(World Coordinate Frame)에서 카메라 좌표계(Camera Coordinate Frame)로 변환하는 과정에서 시작한다. 카메라 외부 파라미터(Extrinsic Parameters)는 로봇에 대한 센서의 위치와 방향을 정의하며, 내부 파라미터(Intrinsic Parameters)는 초점거리(Focal Length), 주점(Principal Point), 픽셀 스케일링(Pixel Scaling), 영상 크기를 정의한다. 이러한 파라미터들은 가시적인 장면의 기하정보가 2차원 영상 평면(Image Plane)에 어떻게 투영되고 객체가 생성된 영상의 어느 위치에 나타나는지를 결정한다.

핀홀 카메라 모델(Pinhole Camera Model)은 원근 영상(Perspective Imaging)을 표현하기 위한 기본적인 기하학적 근사 방법을 제공한다. 카메라 좌표계에서 표현된 3차원 점은 깊이와 초점거리에 따라 영상 평면으로 투영된다. 따라서 카메라에서 멀리 떨어진 객체는 더 작게 보이며, 평행한 구조물도 거리가 증가함에 따라 시각적으로 수렴할 수 있다. 이러한 단순한 투영 모델은 계산 효율성이 높으며 로봇 인지 분야에서 사용되는 다양한 시뮬레이션 RGB 및 깊이 카메라의 수학적 기반이 된다.

현실적인 RGB 시뮬레이션(Realistic RGB Simulation)은 기하학적 투영에 장면의 외관(Scene Appearance)과 조명(Illumination)을 추가한다. 표면 재질(Surface Material), 텍스처(Texture), 광원(Light Source), 그림자(Shadow), 반사(Reflection), 투명도(Transparency), 환경 조건(Environmental Condition)이 최종 픽셀값에 영향을 준다. 렌더링 엔진(Rendering Engine)은 높은 시뮬레이션 처리량을 위해 래스터화(Rasterization)를 사용하거나 더욱 현실적인 광학 상호작용이 필요한 경우 레이 트레이싱(Ray Tracing)을 사용할 수 있다. 적절한 렌더링 방식은 기능적 인지 시험, 합성 데이터 생성(Synthetic Dataset Generation), 고현실성 시뮬레이션-현실 전이(Sim2Real) 평가 등의 목적에 따라 결정된다.

카메라 내부 파라미터(Camera Intrinsic Parameters)는 시뮬레이션을 실제 제품의 인지 소프트웨어 검증에 사용하는 경우 목표 물리 장치의 특성과 일치하도록 설정해야 한다. 영상의 폭과 높이는 공간적 샘플링(Spatial Sampling)을 결정하며, 초점거리와 센서 기하구조(Sensor Geometry)는 실질적인 시야각을 결정한다. 주점은 영상 좌표계에서 광학 중심(Optical Center)을 나타낸다. 정확한 내부 파라미터 모델링을 통해 실제 로봇 소프트웨어에서 사용하는 것과 동일한 보정 행렬(Calibration Matrix)과 투영 연산(Projection Operation)을 시뮬레이션 카메라 데이터에도 적용할 수 있다.

렌즈 왜곡(Lens Distortion)은 카메라의 시야각이 넓어질수록 더욱 중요해진다. 일반적인 렌즈에서는 방사형 배럴 왜곡(Radial Barrel Distortion)이나 핀쿠션 왜곡(Pincushion Distortion)이 발생할 수 있으며, 광학적 또는 기계적 정렬 오차로 인해 접선 왜곡(Tangential Distortion)이 발생할 수도 있다. 시뮬레이터는 이상적인 투영 좌표에 왜곡 계수(Distortion Coefficient)를 적용하여 영상 측정값을 생성할 수 있다. 이를 통해 실제 하드웨어와 유사한 광학 조건에서 카메라 보정(Camera Calibration), 영상 보정(Image Rectification), 특징 추출(Feature Extraction), 시각적 위치 추정(Visual Localization), 인지 알고리즘을 평가할 수 있다.

깊이 카메라(Depth Camera)는 각 픽셀이 가시광 색상뿐만 아니라 거리 정보를 나타내는 영상을 생성한다. 시뮬레이션 센서의 정의에 따라 깊이는 광축(Optical Axis)을 따라 측정한 거리 또는 카메라 원점(Camera Origin)으로부터의 유클리드 거리(Euclidean Range)를 의미할 수 있다. 이러한 정의는 영상 중심에서 멀어질수록 서로 다른 값을 생성하기 때문에 실제 카메라 및 후속 소프트웨어와 일관되게 유지해야 한다. 깊이 출력은 일반적으로 부동소수점 미터(Floating-Point Meter) 또는 양자화된 정수 단위(Quantized Integer Unit)로 표현된다.

이상적인 깊이(Ideal Depth)는 시뮬레이터의 장면 기하정보 또는 렌더링 깊이 버퍼(Depth Buffer)에서 직접 생성할 수 있지만, 실제 깊이 센서는 훨씬 많은 불확실성을 포함한다. 실용적인 시뮬레이션에서는 거리 의존적 노이즈(Range-Dependent Noise), 양자화(Quantization), 유효하지 않은 픽셀(Invalid Pixel), 최소 및 최대 감지 범위, 경계 아티팩트(Edge Artifact), 측정값 손실 등을 적용할 수 있다. 또한 반사성, 투명성, 어두운 표면 또는 기하학적으로 측정하기 어려운 표면은 시뮬레이션 대상이 되는 실제 센싱 기술에 따라 측정 신뢰도를 감소시킬 수 있다.

깊이 센싱 기술(Depth Sensing Technology)은 서로 다른 물리적 특성을 가지므로 하나의 범용적인 노이즈 모델(Universal Noise Model)로 표현해서는 안 된다. 스테레오 깊이(Stereo Depth)는 영상 대응(Image Correspondence)과 베이스라인 기하구조(Baseline Geometry)에 의존하고, 구조광(Structured Light) 시스템은 투사된 패턴과 관측 가능한 표면 텍스처에 의존하며, 비행시간 카메라(Time-of-Flight Camera)는 변조된 광학 신호를 이용해 거리를 추정한다. 계산 효율성을 위해 공통적인 기하학적 깊이 렌더러를 사용할 수 있지만, 이러한 물리적 차이가 인지 성능에 영향을 미친다면 센서별 열화 모델(Sensor-Specific Degradation Model)이 필요하다.

RGB와 깊이 스트림(Depth Stream)은 로봇 인지에서 RGB-D 센싱(RGB-D Sensing) 형태로 자주 결합된다. 두 측정값은 서로 다른 광학 경로나 가상 카메라 원점에서 생성될 수 있으므로 정확한 외부 파라미터 보정과 영상 정합(Image Registration)이 필요하다. 실제 장치가 정렬된 출력을 제공하는 경우에도 시뮬레이션은 현실적인 해상도, 시야각, 시간 특성, 유효하지 않은 깊이값의 동작을 유지해야 한다. 적절한 RGB-D 시뮬레이션은 객체 인식(Object Recognition), 장애물 감지(Obstacle Detection), 3차원 재구성(3D Reconstruction), 조작(Manipulation), 비주얼 슬램(Visual SLAM) 개발을 지원한다.

어안 카메라(Fisheye Camera)는 매우 넓은 시야각을 전체 영상 영역에서 표준 원근 투영(Standard Perspective Projection)만으로 정확하게 표현하기 어렵기 때문에 다른 투영 모델(Projection Model)이 필요하다. 입사 광선의 각도를 단순한 핀홀 관계식으로 변환하는 대신, 어안 모델은 등거리 투영(Equidistant), 등입체각 투영(Equisolid-Angle), 스테레오그래픽 투영(Stereographic), 직교 투영(Orthographic)과 같은 비선형 방사형 매핑(Nonlinear Radial Mapping)을 사용한다. 선택하는 모델은 실제 렌즈 또는 배치될 인지 소프트웨어가 사용하는 보정 방식과 일치해야 한다.

어안 센싱(Fisheye Sensing)의 주요 장점은 넓은 공간 범위(Wide Spatial Coverage)를 확보할 수 있다는 것이다. 모바일 로봇(Mobile Robot), 자율주행차(Autonomous Vehicle), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid)는 여러 개의 어안 카메라를 이용하여 사각 영역을 줄이고 더 적은 수의 센서로 주변 환경을 관측할 수 있다. 그러나 주변부의 강한 왜곡은 객체의 형상, 크기, 겉보기 움직임을 변화시킨다. 따라서 정확한 어안 시뮬레이션은 서라운드 뷰 인지(Surround-View Perception), 비주얼 오도메트리(Visual Odometry), 위치 추정, 의미론적 분할(Semantic Segmentation), 광각 영상 기반 신경망 개발에 중요하다.

카메라 시뮬레이션은 시간적 동작(Temporal Behavior)도 재현해야 한다. RGB 및 깊이 장치는 제한된 프레임률(Frame Rate)로 동작하며 노출 시간, 처리 지연(Processing Latency), 타임스탬프 오프셋(Timestamp Offset), 프레임 지터(Frame Jitter)를 발생시킬 수 있다. 이동 중인 카메라에서는 모션 블러(Motion Blur)가 발생할 수 있으며, 롤링 셔터(Rolling Shutter) 센서는 영상의 각 행을 조금씩 다른 시간에 노출한다. 이러한 현상은 빠른 로봇 움직임에서 기하학적 불일치를 생성할 수 있으므로 비주얼 오도메트리, 드론(Drone), 보행 로봇(Legged Robot), 고속 자율 시스템에서 시간적 카메라 모델링이 특히 중요하다.

노출(Exposure) 및 영상 형성 파라미터(Image Formation Parameter)는 장면 기하정보와 독립적으로 RGB 영상의 외관에 영향을 준다. 노출 시간, 조리개(Aperture), 센서 감도(Sensor Sensitivity), 동적 범위(Dynamic Range), 화이트 밸런스(White Balance), 톤 매핑(Tone Mapping), 자동 노출(Automatic Exposure) 동작은 밝기와 대비를 변화시킬 수 있다. 포화(Saturation)는 밝은 영역의 정보를 제거할 수 있으며, 조명이 부족하면 어두운 영역의 실질적인 노이즈가 증가한다. 이러한 효과의 모델링은 실내, 실외, 주간, 야간 및 급격하게 변화하는 조명 환경에서 동작해야 하는 인지 시스템에서 중요하다.

노이즈(Noise)는 영상 형성 파이프라인(Image Formation Pipeline)의 여러 단계에 적용할 수 있다. 픽셀 수준 모델(Pixel-Level Model)은 광자와 관련된 샷 노이즈(Shot Noise), 전자적 판독 노이즈(Read Noise), 고정 패턴 변동(Fixed-Pattern Variation), 색상 불확실성(Color Uncertainty), 압축 아티팩트(Compression Artifact), 양자화 등을 근사할 수 있다. 단순화된 시뮬레이터에서는 렌더링 이후 보정된 가우시안 노이즈(Gaussian Noise) 또는 신호 의존적 노이즈(Signal-Dependent Noise)를 적용할 수 있다. 적절한 복잡성 수준은 목표 알고리즘이 장면 의미, 기하학적 정확도, 저조도 영상 또는 세부적인 영상 특성 중 무엇에 민감한지에 따라 결정된다.

환경 효과(Environmental Effect)는 시뮬레이션 카메라 측정값에 추가적인 영향을 준다. 비(Rain), 안개(Fog), 먼지(Dust), 눈부심(Glare), 렌즈 오염(Lens Contamination), 그림자, 반사, 부분 가림(Partial Occlusion)은 RGB와 깊이 정보를 동시에 또는 서로 다른 방식으로 저하시킬 수 있다. 이러한 효과를 단순한 시각적 장식으로 처리하기보다 감지 확률과 측정 품질을 변화시키는 센싱 조건(Sensing Condition)으로 모델링할 수 있다. 이러한 시나리오는 통제된 실험실이나 공장 외부에서 동작해야 하는 자율 로봇을 시험하는 데 유용하다.

카메라 배치(Camera Placement) 역시 중요한 시뮬레이션 파라미터이다. 센서 높이, 방향, 베이스라인(Baseline), 장착 각도, 기계적 공차(Mechanical Tolerance)는 관측 가능한 영역을 결정하고 후속 인지 정확도에 영향을 준다. 조립 공차 또는 보정 오차를 표현하기 위해 외부 파라미터에 작은 변동을 적용할 수 있다. 다중 카메라 시스템(Multi-Camera System)에서는 시야각의 중첩과 상대적인 카메라 자세를 신중하게 모델링해야 하며, 이는 스테레오 추정(Stereo Estimation), 파노라마 인지(Panoramic Perception), 다중 시점 기하(Multi-View Geometry), 센서 융합(Sensor Fusion)에 직접적인 영향을 미친다.

합성 데이터 생성(Synthetic Data Generation)은 렌더링된 장면으로부터 RGB 영상과 함께 현실에서 얻기 어렵거나 높은 비용이 필요한 정확한 정답 데이터(Ground Truth)를 제공할 수 있기 때문에 카메라 시뮬레이션의 큰 이점을 얻는다. 동일한 장면 상태로부터 깊이, 의미론적 라벨(Semantic Label), 인스턴스 마스크(Instance Mask), 옵티컬 플로(Optical Flow), 표면 법선(Surface Normal), 객체 자세(Object Pose), 바운딩 박스(Bounding Box)를 생성할 수 있다. 이후 외관과 카메라 파라미터를 무작위화하여 모든 관측값을 수작업으로 주석 처리하지 않고도 로봇 인지 모델의 학습 및 평가를 위한 다양한 데이터셋을 생성할 수 있다.

도메인 랜덤화(Domain Randomization)는 시뮬레이션 에피소드마다 카메라 자세, 초점 파라미터, 노출, 조명, 텍스처, 노이즈, 왜곡, 깊이 불확실성, 환경 조건을 변화시킬 수 있다. 목표는 하나의 카메라 구성을 완벽하게 재현하는 것이 아니라 학습 시스템을 현실적으로 가능한 다양한 관측값의 분포에 노출하는 것이다. 적절하게 설정된 랜덤화 범위(Randomization Range)는 합성 영상의 특정 아티팩트에 대한 의존도를 감소시키고 인지 모델 또는 학습된 로봇 정책(Learned Robot Policy)을 실제 카메라로 전이할 때 강건성(Robustness)을 향상시킬 수 있다.

검증(Validation)은 통제되고 대표적인 장면에서 시뮬레이션 카메라와 실제 장치를 비교하는 방식으로 수행해야 한다. RGB 비교에서는 기하구조, 시야각, 왜곡, 밝기 분포(Brightness Distribution), 특징점의 동작을 평가할 수 있으며, 깊이 검증에서는 거리 오차, 데이터 손실 패턴(Missing-Data Pattern), 경계 영역의 동작, 거리에 따른 불확실성을 분석할 수 있다. 어안 카메라 검증에서는 영상 전체 영역에 걸친 각도 투영 정확도(Angular Projection Accuracy)를 추가로 평가해야 한다. 후속 인지 알고리즘의 성능은 시뮬레이션 충실도가 충분한지를 판단하는 중요한 시스템 수준의 평가 기준을 제공한다.

실용적인 카메라 시뮬레이션 아키텍처(Camera Simulation Architecture)는 투영 기하(Projection Geometry), 보정된 내부 및 외부 파라미터, 렌더링, 광학 왜곡(Optical Distortion), 깊이 생성(Depth Generation), 시간적 동작, 노이즈, 환경 효과를 통합한다. 모든 실험에서 최대 수준의 현실성을 적용하기보다 개발 목적에 따라 충실도(Fidelity)를 단계적으로 높일 수 있다. 제어 가능한 불확실성과 함께 일관된 RGB, 깊이, 어안 관측값을 제공함으로써 카메라 시뮬레이션은 인지 시스템 개발, 합성 데이터 생성, 검증, 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 핵심 도구가 된다.

## 04.03. LiDAR Point Cloud Simulation Ray Casting [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

라이다 포인트 클라우드 시뮬레이션(LiDAR Point Cloud Simulation)은 레이저 스캐너(Laser Scanner)가 가상의 3차원 환경을 관측할 때 생성하는 기하학적 측정값을 재현한다. 시뮬레이터는 완전한 장면 기하정보(Scene Geometry)를 인지 소프트웨어에 직접 제공하는 대신 센서의 스캐닝 패턴(Scanning Pattern)에 따라 개별 거리 측정값을 생성한다. 이러한 측정값은 실제 물리 라이다 하드웨어의 출력과 유사한 포인트 클라우드(Point Cloud)로 변환되며 위치 추정(Localization), 매핑(Mapping), 객체 감지(Detection), 내비게이션(Navigation) 알고리즘에서 사용할 수 있다.

대부분의 라이다 시뮬레이션(LiDAR Simulation)을 구성하는 기본 메커니즘은 레이 캐스팅(Ray Casting)이다. 가상의 광선(Virtual Ray)은 시뮬레이션된 라이다 위치에서 시작하여 지정된 방향으로 환경을 통과한다. 시뮬레이션 엔진(Simulation Engine)은 광선이 객체와 교차하는지를 판단하고 가장 가까운 유효 교차점(Intersection)을 찾는다. 센서 원점과 이 교차점 사이의 거리는 이상적인 거리 측정값(Ideal Range Measurement)이 되며, 교차점 좌표는 이에 대응하는 3차원 점을 제공한다.

광선(Ray)은 수학적으로 원점(Origin)과 정규화된 방향 벡터(Normalized Direction Vector)를 사용하여 표현할 수 있다. 광선상의 점은 원점에서 스칼라 거리(Scalar Distance)를 증가시키면서 계산된다. 장면 객체는 기하학적 기본 형상(Geometric Primitive), 폴리곤 메시(Polygon Mesh), 충돌 기하구조(Collision Geometry), 가속 구조(Acceleration Structure)를 이용하여 표현된다. 레이 캐스팅 엔진은 광선과 이러한 표현 사이의 교차를 계산하고 일반적으로 설정된 센싱 조건을 만족하는 가장 가까운 가시 표면을 선택한다.

완전한 라이다 스캔(LiDAR Scan)을 생성하려면 실제 센서의 스캐닝 기하구조(Scanning Geometry)에 따라 배치된 다수의 광선이 필요하다. 2차원 라이다(2D LiDAR)는 수평 각도 범위에서 광선을 주사할 수 있으며, 다중 채널 3차원 라이다(Multi-Channel 3D LiDAR)는 여러 개의 수직 방향 레이저 채널을 사용하면서 수평으로 회전한다. 솔리드 스테이트(Solid-State) 장치는 서로 다른 비반복적 또는 구조화된 스캐닝 패턴을 사용할 수 있다. 따라서 정확한 시뮬레이션을 위해서는 최대 거리와 해상도뿐만 아니라 실제 각도 샘플링 패턴(Angular Sampling Pattern)도 모델링해야 한다.

회전형 다중 빔 라이다(Rotating Multi-Beam LiDAR)에서는 각각의 레이저 채널이 사전에 정의된 수직 각도를 가지며 센서는 수평 방위각(Horizontal Azimuth)을 지속적으로 변화시킨다. 한 번의 회전 동안 모든 채널의 반환값(Return)을 결합하면 3차원 포인트 클라우드가 생성된다. 16, 32, 64개 이상의 채널을 사용하는 센서는 서로 다른 수직 샘플링 밀도(Vertical Sampling Density)를 생성한다. 포인트 클라우드의 희소성(Sparsity)은 객체 감지, 지형 인지(Terrain Perception), 위치 추정 성능에 직접 영향을 주므로 시뮬레이터는 이러한 채널 배열을 재현해야 한다.

거리 측정값을 직교 좌표계 점(Cartesian Point)으로 변환하려면 측정 거리와 수평 및 수직 빔 각도(Beam Angle)를 함께 사용한다. 각각의 반환점은 먼저 라이다 좌표계(LiDAR Coordinate Frame)에서 표현되며 이후 센서의 외부 파라미터 보정(Extrinsic Calibration)을 사용하여 로봇, 오도메트리(Odometry), 지도(Map), 월드 좌표계(World Frame)로 변환할 수 있다. 작은 장착 오차도 라이다 관측값과 다른 센서 측정값 사이에 체계적인 위치 편차를 발생시킬 수 있으므로 정확한 좌표 변환이 중요하다.

최대 및 최소 감지 거리(Maximum and Minimum Sensing Range)는 어떤 광선 교차점이 유효한 반환값을 생성할 수 있는지를 정의한다. 최소 거리보다 가까운 객체는 사각 영역(Blind Region)에 위치할 수 있으며 최대 거리보다 먼 교차점은 일반적으로 제거되어야 한다. 각도 해상도(Angular Resolution)는 인접한 광선 사이의 간격을 결정하며 결과적으로 생성되는 포인트 클라우드의 밀도를 결정한다. 특정 물리 라이다 구성을 재현하는 것이 목적이라면 이러한 파라미터를 대상 하드웨어와 일치시켜야 한다.

이상적인 레이 캐스팅은 완벽하게 정확한 교차점을 생성하지만 실제 라이다 측정값에는 불확실성(Uncertainty)이 존재한다. 센서 사양이나 실험 측정값으로부터 도출된 통계 모델(Statistical Model)을 이용하여 각각의 반환값에 거리 노이즈(Range Noise)를 추가할 수 있다. 오차 크기는 거리, 입사각(Incidence Angle), 표면 특성(Surface Property), 환경 조건에 따라 달라질 수 있다. 또한 무작위 각도 섭동(Random Angular Perturbation)을 적용하여 빔 방향의 불확실성을 표현할 수 있으며 측정 거리가 증가할수록 더 큰 공간 오차가 발생하도록 모델링할 수 있다.

표면 반사율(Surface Reflectivity)은 실제 레이저 반환값에 큰 영향을 준다. 반사율이 높은 재질은 강한 측정 신호를 생성할 수 있지만 어둡거나 반사율이 낮은 표면에서는 감지 확률(Detection Probability)이 감소할 수 있다. 유리와 같은 투명 또는 정반사 재질(Transparent or Specular Material)은 측정값 손실이나 위치가 변형된 반환값 또는 예상하지 못한 반환값을 발생시킬 수 있다. 고충실도 시뮬레이터(High-Fidelity Simulator)는 장면 표면에 광학적 또는 라이다 전용 재질 특성을 부여하고 이를 이용하여 반환 확률, 강도(Intensity), 측정 불확실성을 변경할 수 있다.

레이저 빔과 표면 사이의 입사각도 측정 품질에 영향을 준다. 표면 법선(Surface Normal)에 거의 수직으로 입사하는 광선은 일반적으로 스치듯 입사하는 광선보다 강하고 안정적인 반환값을 생성한다. 작은 입사각 조건에서는 반사 에너지가 신뢰할 수 있는 감지에 충분하지 않을 수 있다. 각도 의존적 반환 확률(Angle-Dependent Return Probability)을 모델링하면 차량 차체, 건물 표면, 식생(Vegetation), 도로 경계 및 기타 복잡한 구조물에서 발생하는 희소한 측정 특성을 재현하는 데 도움이 된다.

라이다 강도(LiDAR Intensity)는 기하학적 거리 이상의 정보를 제공한다. 실제 센서는 반사된 광학 에너지와 관련된 반환 신호 강도(Return Strength)를 제공할 수 있지만 정확한 의미와 스케일은 장치에 따라 달라진다. 시뮬레이션 강도는 표면 반사율, 거리, 입사각, 센서 특성을 이용하여 추정할 수 있다. 단순화된 강도 모델이 실제 하드웨어를 완벽하게 재현할 수는 없지만 반사도(Reflectance)를 위치 추정, 분류(Classification), 특징 추출(Feature Extraction)에 활용하는 알고리즘을 지원할 수 있다.

실제 라이다 센서는 모든 포인트를 동시에 캡처하는 것이 아니라 일정 시간에 걸쳐 스캔한다. 회전 스캔(Rotating Scan)이 진행되는 동안 로봇이나 주변 객체는 첫 번째 측정과 마지막 측정 사이에서 상당히 이동할 수 있다. 따라서 전체 포인트 클라우드를 순간적으로 획득된 하나의 관측값으로 처리하면 비현실적인 데이터가 생성될 수 있다. 고충실도 시뮬레이션에서는 개별 광선 또는 광선 그룹에 획득 타임스탬프(Acquisition Timestamp)를 연결하여 로봇과 객체의 움직임에 따른 모션 왜곡(Motion Distortion)이 자연스럽게 발생하도록 한다.

모션 왜곡은 자율주행차(Autonomous Vehicle), 실외 자율이동로봇(Outdoor AMR), 무인항공기(UAV) 및 기타 고속 이동 플랫폼에서 특히 중요하다. 이동하는 로봇에 장착된 회전형 라이다는 한 번 회전하는 동안 조금씩 다른 자세(Pose)에서 환경의 각 영역을 관측한다. 각 빔의 타임스탬프에 따라 센서 자세를 갱신함으로써 이러한 효과를 시뮬레이션할 수 있다. 이를 통해 디스큐잉(Deskewing) 및 모션 보상(Motion Compensation) 알고리즘을 실제 동작과 유사한 조건에서 평가할 수 있다.

환경 효과(Environmental Effect)는 라이다 측정 성능을 추가로 저하시킬 수 있다. 안개(Fog), 비(Rain), 눈(Snow), 먼지(Dust), 공기 중 입자(Airborne Particle)는 레이저 에너지를 감쇠시키거나 빔이 목표 표면에 도달하기 전에 원하지 않는 반환값을 생성할 수 있다. 정교한 광학 시뮬레이션은 이러한 상호작용을 물리적으로 모델링할 수 있지만 실용적인 로봇 시뮬레이터에서는 거리 의존적 드롭아웃(Range-Dependent Dropout), 무작위 허위 반환(Random False Return), 감지 확률 감소 등으로 근사하는 경우가 많다. 필요한 복잡성은 목표 운용 환경과 검증 목적에 따라 결정된다.

가림(Occlusion)은 일반적으로 가장 먼저 보이는 교차점만 반환되기 때문에 레이 캐스팅 과정에서 자연스럽게 발생한다. 따라서 벽, 차량, 사람, 식생 또는 기타 장애물 뒤에 위치한 객체는 부분적으로 또는 완전히 보이지 않게 된다. 이러한 특성으로 인해 광선 기반 시뮬레이션(Ray-Based Simulation)은 현실적인 인지 시험에 유용하다. 그러나 일부 실제 라이다 시스템은 빔이 식생과 같은 반투명 또는 공간적으로 분포된 구조물과 상호작용할 때 여러 개의 반환값을 제공할 수 있으므로 더욱 정교한 다중 반환 모델링(Multi-Return Modeling)이 필요할 수 있다.

포인트 클라우드에는 측정값 손실(Missing Measurement)이 자주 발생한다. 시뮬레이션 라이다에서는 거리, 재질, 입사각, 환경 조건 또는 보정된 드롭아웃 확률(Dropout Probability)에 따라 반환값을 제거하여 이러한 현상을 표현할 수 있다. 다중경로 반사(Multipath Reflection)나 예상하지 못한 측정값을 모델링하기 위해 추가적인 이상치(Outlier)를 생성할 수도 있다. 이러한 불완전성은 완전하고 노이즈가 없는 포인트 클라우드만을 이용하여 학습하거나 검증한 알고리즘이 희소하고 불규칙한 실제 측정 데이터에서 성능 저하를 보일 수 있기 때문에 중요하다.

장면 기하구조(Scene Geometry)의 품질은 라이다 시뮬레이션 충실도에 직접적인 영향을 준다. 렌더링을 위해 최적화된 시각적 메시(Visual Mesh)는 불필요한 세부 구조를 포함할 수 있는 반면 단순화된 충돌 메시(Collision Mesh)는 레이저 인지에 중요한 구조물을 제거할 수 있다. 얇은 기둥, 연석(Curb), 울타리, 식생, 케이블, 작은 장애물은 카메라 영상에서 차지하는 영역이 작더라도 내비게이션에 영향을 줄 수 있다. 따라서 레이 캐스팅에 사용하는 기하학적 표현은 목표 라이다 작업에 중요한 구조물을 보존해야 한다.

효율적인 레이 캐스팅은 하나의 라이다 스캔에서도 수만 개에서 수십만 개의 교차 질의(Intersection Query)가 필요할 수 있기 때문에 매우 중요하다. 경계 볼륨 계층구조(Bounding Volume Hierarchy)와 같은 공간 가속 구조(Spatial Acceleration Structure)는 각각의 광선에 필요한 기하구조 검사 횟수를 줄여준다. 최신 시뮬레이션 엔진은 중앙처리장치(CPU), 그래픽처리장치(GPU), 하드웨어 가속 레이 트레이싱 장치(Hardware-Accelerated Ray-Tracing Unit)를 이용하여 대규모 광선 배치를 병렬로 처리할 수 있다. 이를 통해 실시간 또는 실시간보다 빠른 실행 속도를 유지하면서 고밀도 라이다 시뮬레이션을 수행할 수 있다.

그래픽처리장치 가속(GPU Acceleration)은 합성 데이터 생성(Synthetic Data Generation)과 대규모 병렬 로봇 시뮬레이션(Massively Parallel Robot Simulation)에서 특히 중요하다. 강화학습(Reinforcement Learning), 인지 모델 학습, 대규모 검증 과정에서는 수천 개의 가상 센서가 동시에 포인트 클라우드를 생성해야 할 수 있다. 배치 레이 질의(Batched Ray Query)와 병렬 장면 탐색(Parallel Scene Traversal)을 이용하면 순차적인 CPU 레이 캐스팅보다 높은 시뮬레이션 처리량을 확보할 수 있다. 사용 가능한 계산 자원에 따라 충실도와 광선 밀도 역시 조절할 수 있다.

라이다 시뮬레이션은 생성된 포인트 클라우드와 함께 정확한 정답 정보(Ground Truth)를 제공할 수 있다. 각각의 반환점에 의미론적 클래스(Semantic Class), 인스턴스 식별자(Instance Identifier), 표면 법선, 객체 속도(Object Velocity) 또는 기타 장면 메타데이터(Scene Metadata)를 연결할 수 있다. 이러한 주석 정보는 실제 라이다 데이터에서 수작업으로 생성하기 어렵고 비용도 많이 든다. 따라서 합성 포인트 클라우드는 3차원 객체 감지(3D Object Detection), 의미론적 분할(Semantic Segmentation), 점유 추정(Occupancy Estimation), 장면 이해(Scene Understanding) 모델의 학습과 평가를 지원할 수 있다.

도메인 랜덤화(Domain Randomization)는 빔 노이즈, 거리 한계, 각도 오프셋(Angular Offset), 센서 장착 자세, 표면 특성, 드롭아웃 확률, 날씨, 환경 기하구조를 변화시킬 수 있다. 하나의 완벽하게 설정된 가상 라이다만을 사용하여 인지 알고리즘을 학습시키는 대신 다양한 현실적 센서 동작 분포를 경험하도록 한다. 이러한 접근 방법은 하드웨어 편차(Hardware Variation), 보정 불확실성(Calibration Uncertainty), 환경 변화, 시뮬레이션 센서와 실제 센서 사이의 차이에 대한 강건성(Robustness)을 향상시킬 수 있다.

검증(Validation)은 동일하거나 동등한 장면에서 수집한 시뮬레이션 포인트 클라우드와 실제 포인트 클라우드를 비교하는 방식으로 수행해야 한다. 주요 비교 특성에는 거리 오차 분포(Range-Error Distribution), 포인트 밀도(Point Density), 각도 구조(Angular Structure), 드롭아웃 패턴(Dropout Pattern), 강도 특성(Intensity Behavior), 모션 왜곡, 거리 및 표면 종류에 따른 감지 확률이 포함된다. 슬램(SLAM), 위치 추정, 장애물 감지, 3차원 인지 성능과 같은 후속 시스템의 결과도 시뮬레이션 센서가 목표 용도에 충분한 충실도를 갖추었는지를 판단하는 시스템 수준의 근거를 제공할 수 있다.

실용적인 라이다 시뮬레이션 파이프라인(LiDAR Simulation Pipeline)은 장면 기하구조, 센서 자세, 빔 구성(Beam Configuration), 레이 캐스팅, 교차점 처리(Intersection Processing), 노이즈 모델링, 시간 특성, 환경 효과, 좌표 변환(Coordinate Transformation)을 연결한다. 모든 부분에서 복잡성을 최대화하기보다 목표 알고리즘에 실질적으로 영향을 주는 요소를 중심으로 충실도를 높여야 한다. 적절하게 구성된 광선 기반 라이다 시뮬레이션은 현실적인 포인트 클라우드를 제공하여 인지 시스템 개발, 합성 데이터 생성, 자율 시스템 검증(Autonomy Validation), 시뮬레이션-현실 전이(Sim2Real Transfer)를 지원한다.

## 04.04. Radar Simulation Doppler Range RCS Modeling [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

레이더 시뮬레이션(Radar Simulation)은 무선 주파수 센싱 시스템(Radio-Frequency Sensing System)이 전자기파(Electromagnetic Wave)를 송신하고 반사된 신호를 분석하여 객체를 관측하는 방식을 재현한다. 주로 외관을 측정하는 카메라(Camera)나 광학 펄스를 통해 기하학적 거리를 측정하는 라이다(LiDAR)와 달리, 레이더는 거리와 상대 방사 속도(Relative Radial Velocity)에 대한 정보를 직접 제공할 수 있다. 이러한 특성으로 인해 레이더는 자율주행차, 실외 로봇, 무인항공기(UAV), 가시성이 낮은 환경에서 동작하는 시스템에 특히 유용하다.

기본적인 레이더 측정(Radar Measurement)은 센서가 환경을 향해 전자기 신호를 송신하는 과정에서 시작된다. 이 에너지의 일부는 객체와 상호작용한 후 에코(Echo) 형태로 수신기에 돌아온다. 송신과 수신 사이의 전파 지연(Propagation Delay)에는 목표물까지의 거리 정보가 포함되며, 반환 신호의 주파수 또는 위상 변화는 상대적인 움직임을 나타낼 수 있다. 레이더 시뮬레이션은 실제 무선 주파수 하드웨어 없이 이러한 측정 과정을 재현하는 것을 목표로 한다.

거리 추정(Range Estimation)은 전자기 에너지가 레이더에서 목표물까지 이동한 후 다시 돌아오는 데 필요한 시간을 기반으로 한다. 단순한 펄스 레이더(Pulse Radar)의 경우 목표물 거리는 전파 시간에 빛의 속도를 곱한 값의 절반으로 표현할 수 있다. 절반을 사용하는 이유는 신호가 왕복 이동하기 때문이다. 실제 시뮬레이션에서는 목표물의 기하구조와 전파 경로(Propagation Path)를 이용하여 지연시간을 결정하고, 이를 기반으로 이상적인 거리 측정값을 생성할 수 있다.

많은 최신 로봇 및 자동차용 레이더는 주파수 변조 연속파(Frequency-Modulated Continuous-Wave), 즉 FMCW 기술을 사용한다. FMCW 레이더는 개별적인 펄스를 송신하는 대신 정의된 파형 또는 처프(Chirp)에 따라 송신 주파수를 연속적으로 변화시킨다. 송신 신호와 수신 신호의 차이는 목표물 거리와 관련된 비트 주파수(Beat Frequency)를 생성한다. 시뮬레이션에서는 직접적인 목표물 측정부터 상세한 파형 생성 및 신호 처리까지 다양한 추상화 수준에서 이러한 동작을 모델링할 수 있다.

거리 분해능(Range Resolution)은 비슷한 거리에 위치한 두 개의 목표물을 레이더가 구별할 수 있는 능력을 의미한다. 이는 단순히 최대 감지 거리보다 송신 신호의 대역폭(Bandwidth)에 크게 의존한다. 인지 시스템 개발을 위한 시뮬레이션에서는 측정값을 거리 빈(Range Bin)으로 그룹화하거나 양자화하여 유한한 거리 분해능을 재현해야 한다. 그렇지 않으면 서로 가까이 있는 가상의 목표물들이 실제 레이더 하드웨어보다 비현실적으로 쉽게 분리되어 보일 수 있다.

도플러 효과(Doppler Effect)는 레이더가 제공하는 가장 중요한 기능 중 하나이다. 레이더와 목표물 사이의 상대 거리가 변화하면 반사된 전자기파에서 주파수 이동(Frequency Shift)이 발생한다. 접근하는 목표물과 멀어지는 목표물은 서로 다른 도플러 특성(Doppler Signature)을 생성한다. 측정된 도플러 주파수는 방사 속도(Radial Velocity)로 변환할 수 있으므로 레이더는 연속 프레임 사이의 객체 위치 차이만을 이용하지 않고 움직임을 직접 추정할 수 있다.

레이더는 센서에서 목표물로 향하는 방향 성분의 속도인 방사 속도(Radial Velocity)를 측정한다. 따라서 레이더의 시선 방향(Line of Sight)에 수직으로 빠르게 움직이는 객체는 실제 속도가 높더라도 작은 도플러 속도를 나타낼 수 있다. 이러한 기하학적 한계는 시뮬레이션에서도 유지되어야 한다. 현실적인 도플러 모델링(Doppler Modeling)은 차량, 보행자, 이동 로봇 및 기타 동적 객체의 운동 상태를 추정하는 데 특히 중요하다.

거리 및 도플러 측정값은 흔히 2차원 거리-도플러 표현(Range-Doppler Representation)으로 구성된다. 푸리에 변환(Fourier Transform)과 같은 신호 처리 단계는 전파 지연과 도플러 주파수에 따라 반사 에너지를 분리한다. 이 표현에서 피크(Peak)는 특정 거리와 방사 속도에 존재할 가능성이 있는 목표물을 나타낸다. 고충실도 레이더 시뮬레이터(High-Fidelity Radar Simulator)는 중간 신호 표현을 생성할 수 있으며, 계산 효율성을 중시하는 시뮬레이터는 동일한 거리 및 속도 특성을 가진 감지 결과를 직접 생성할 수 있다.

각도 정보(Angular Information)를 이용하면 레이더가 센서를 기준으로 목표물의 위치를 추정할 수 있다. 다중 안테나 배열(Multi-Antenna Array)은 수신 요소 사이의 위상 차이(Phase Difference)를 비교하여 방위각(Azimuth)을 추정하고, 보다 발전된 시스템에서는 고도각(Elevation)까지 추정한다. 구현 가능한 각도 분해능(Angular Resolution)은 안테나 기하구조, 파장(Wavelength), 신호 처리 방식, 센서 설계에 따라 달라진다. 따라서 시뮬레이션 레이더는 정확한 3차원 객체 위치를 그대로 제공하는 대신 현실적인 시야각과 각도 불확실성(Angular Uncertainty)을 포함해야 한다.

레이더 단면적(Radar Cross Section), 즉 RCS는 객체가 레이더 에너지를 수신기 방향으로 얼마나 강하게 반사하는지를 나타낸다. RCS는 단순히 객체의 물리적인 크기와 동일한 개념이 아니다. 목표물의 형상, 재질 특성, 레이더 주파수, 편파(Polarization), 관측 방향, 표면 방향에 따라 달라진다. 큰 객체라도 특정 조건에서는 약한 반환 신호를 생성할 수 있으며, 반대로 작은 금속 구조물이 강한 반사를 생성할 수도 있다. 따라서 RCS 모델링은 현실적인 레이더 시뮬레이션의 핵심 요소이다.

단순화된 레이더 시뮬레이터는 각각의 객체 클래스(Object Class) 또는 재질에 일정한 RCS 값을 할당할 수 있다. 보다 발전된 모델에서는 관측 각도(Aspect Angle)와 개별 표면 기하구조에 따라 RCS를 변화시킨다. 이에 따라 차량 차체, 금속 기반 시설, 건물 외벽, 식생(Vegetation), 보행자, 도로 객체가 서로 다른 반환 신호 강도를 생성할 수 있다. 이러한 모델링은 레이더를 장착한 로봇이 복잡한 실내외 환경을 이동할 때 발생하는 불규칙한 감지 패턴을 재현하는 데 도움이 된다.

수신되는 레이더 전력(Received Radar Power)은 전파 거리가 증가함에 따라 크게 감소한다. 레이더 모델링에서는 일반적으로 레이더 거리 방정식(Radar Range Equation)을 통해 송신 전력, 안테나 이득(Antenna Gain), 파장, 목표물 RCS, 전파 거리, 수신기 특성을 고려한다. 시뮬레이터가 완전한 무선 주파수 신호 체인을 재현하지 않는 경우에도 이러한 관계를 근사하여 목표물이 감지 가능한 반환 신호를 생성하는지와 해당 신호의 강도를 결정할 수 있다.

따라서 감지(Detection)는 단순한 기하학적 가시성(Geometric Visibility)과 동일하지 않다. 목표물이 레이더 시야각 내부에 있더라도 반사 신호가 감지 임계값(Detection Threshold)보다 낮으면 측정값이 생성되지 않을 수 있다. 반대로 반사 특성이 강한 구조물은 상당히 먼 거리에서도 감지될 수 있다. 시뮬레이터에서는 기하학적으로 보이는 모든 객체를 반환하는 대신 신호 대 잡음비(Signal-to-Noise Ratio), 감지 임계값, 확률적 감지(Probabilistic Detection), 거리 의존적 감쇠(Range-Dependent Attenuation)를 이용하여 이러한 특성을 모델링할 수 있다.

노이즈(Noise)와 클러터(Clutter)는 레이더 시뮬레이션에서 필수적인 요소이다. 수신기 전자회로는 열 잡음(Thermal Noise)과 전자적 노이즈를 발생시키며, 주변 환경에서는 도로, 벽, 식생, 지형, 건물 및 기타 표면으로부터 반사가 발생한다. 이러한 원하지 않는 신호는 실제 목표물의 반환 신호와 중첩될 수 있다. 노이즈와 클러터를 모델링하면 인지 알고리즘이 비현실적으로 깨끗한 레이더 관측값 대신 오탐(False Detection), 불확실한 측정값, 변화하는 신호 품질을 경험하도록 할 수 있다.

다중경로 전파(Multipath Propagation)는 레이더 에너지가 하나 이상의 경로를 통해 수신기에 도달할 때 발생한다. 지면, 벽, 차량 또는 대형 금속 구조물에서 발생하는 반사는 고스트 목표물(Ghost Target)이나 왜곡된 측정값을 생성할 수 있다. 도심 도로, 공장, 창고, 항만, 주차 구조물에서는 강한 다중경로 현상이 발생할 수 있다. 고충실도 시뮬레이션에서는 여러 전자기 전파 경로를 추적할 수 있으며, 단순한 모델에서는 보정된 오탐 또는 측정값 섭동(Measurement Perturbation)을 추가하여 이를 근사할 수 있다.

레이더에서의 가림(Occlusion)은 전자기파가 주파수와 물리적 특성에 따라 재질과 상호작용하기 때문에 순수한 광학 센싱과 다르다. 일부 객체는 레이더 에너지를 강하게 차단하거나 반사하지만 다른 구조물에서는 부분적인 투과 또는 복잡한 다중경로 전파가 발생할 수 있다. 단순한 기하학적 레이 모델(Geometric Ray Model)을 통해 직접 가시성을 근사할 수 있지만, 고급 레이더 시뮬레이션에서는 복잡한 환경을 재현하기 위해 재질을 고려한 반사, 투과(Transmission), 산란(Scattering), 다중 반사(Multiple-Bounce Propagation)가 필요할 수 있다.

기상 효과(Weather Effect)는 일반적으로 카메라와 광학 라이다가 경험하는 영향과 다르게 나타난다. 레이더는 광학 센싱 성능을 크게 저하시키는 어둠, 안개, 비 등의 환경에서도 동작할 수 있지만 심한 강수와 대기 조건은 여전히 신호 감쇠와 클러터에 영향을 줄 수 있다. 따라서 실외 자율주행을 위한 레이더 시뮬레이션에서는 기상 조건에 완전히 영향을 받지 않는 센서로 가정하기보다 레이더의 상대적인 강건성(Robustness)과 남아 있는 성능 저하 메커니즘을 함께 표현해야 한다.

레이더 측정값은 시간에 따라 생성되므로 현실적인 시간적 동작(Temporal Behavior)을 포함해야 한다. 프레임률(Frame Rate), 처프 지속시간(Chirp Duration), 처리 지연(Processing Latency), 타임스탬프 정확도(Timestamp Accuracy), 통신 지연(Communication Delay)은 센서 융합(Sensor Fusion)에 영향을 줄 수 있다. 레이더 데이터를 카메라, 라이다, 위성항법시스템(GNSS), 관성 측정 장치(IMU)와 결합할 때 시간적 정렬 오차는 객체 위치와 속도의 불일치를 발생시킬 수 있다. 따라서 추적 및 다중 센서 인지 시스템을 평가할 때 센서의 시간 특성을 시뮬레이션에 유지해야 한다.

실제 레이더 출력(Physical Radar Output)은 여러 추상화 수준(Abstraction Level)에서 표현할 수 있다. 저수준 시뮬레이터는 복소 동상 및 직교 위상 샘플(Complex In-Phase and Quadrature Samples), 거리 프로파일(Range Profile), 거리-도플러 맵(Range-Doppler Map)과 같은 원시 또는 중간 신호를 생성할 수 있다. 고수준 시뮬레이터는 거리, 방위각, 고도각, 방사 속도, RCS, 신뢰도(Confidence)를 포함하는 목표물 감지 결과를 생성할 수 있다. 추상화 수준의 선택은 전자기적 충실도, 계산 비용, 개발 대상 알고리즘의 요구사항 사이의 균형을 고려해야 한다.

레이더 포인트 클라우드(Radar Point Cloud)는 라이다 포인트 클라우드(LiDAR Point Cloud)와 상당히 다르다. 일반적으로 훨씬 희소하며 가시적인 객체 표면의 직접적인 샘플이 아니라 전자기 산란 중심(Electromagnetic Scattering Center)을 나타낸다. 차량 하나에서도 몇 개의 강한 레이더 감지만 생성될 수 있으며 그 위치가 차량의 기하학적 경계와 정확하게 일치하지 않을 수 있다. 따라서 인지 알고리즘은 기하구조에서 직접 생성한 고밀도 포인트 클라우드가 아니라 현실적인 희소성과 산란 특성을 유지하는 레이더 관측값을 이용하여 시험해야 한다.

동적 객체 시뮬레이션(Dynamic-Object Simulation)은 도플러 정보가 레이더의 주요 장점 중 하나이기 때문에 특히 중요하다. 각 이동 객체의 속도를 레이더 시선 방향으로 투영하여 예상되는 방사 속도를 계산해야 한다. 또한 로봇 자체의 움직임(Ego-Motion)도 포함해야 하는데, 레이더 플랫폼이 이동하면 정지된 기반 시설도 0이 아닌 상대 속도를 가진 것처럼 나타날 수 있기 때문이다. 정확한 운동 모델링(Motion Modeling)은 현실적인 추적(Tracking), 속도 추정(Velocity Estimation), 이동 객체 분류(Moving-Object Classification)를 지원한다.

합성 레이더 데이터(Synthetic Radar Data)는 실제 센서에서 얻기 어려운 정답 정보(Ground Truth)를 포함할 수 있다. 시뮬레이션된 감지 결과에는 객체 식별자(Object Identifier), 의미론적 클래스(Semantic Class), 실제 위치, 속도, RCS 값, 전파 경로 등의 정보를 연결할 수 있다. 이러한 주석 정보는 레이더 객체 감지(Radar Object Detection), 추적, 점유 추정(Occupancy Estimation), 센서 융합, 머신러닝 시스템의 개발을 지원하며 희소하고 모호한 실제 레이더 측정값을 수작업으로 라벨링하는 비용을 줄일 수 있다.

도메인 랜덤화(Domain Randomization)는 목표물 RCS, 감지 확률, 거리 노이즈(Range Noise), 도플러 노이즈(Doppler Noise), 각도 불확실성, 클러터 밀도(Clutter Density), 다중경로 특성, 날씨, 센서 자세(Sensor Pose), 감지 임계값 등을 변화시킬 수 있다. 학습 알고리즘을 다양한 현실적인 레이더 특성에 노출하면 하나의 이상적인 센서 구성에 대한 의존도를 낮출 수 있다. 또한 랜덤화를 통해 실제 시험에서 체계적으로 재현하기 어려운 제조 편차(Manufacturing Variation)와 환경 불확실성을 표현할 수 있다.

검증(Validation)은 동일하거나 동등한 시나리오에서 시뮬레이션 레이더와 실제 레이더의 측정값을 비교하는 방식으로 수행해야 한다. 주요 특성에는 거리 정확도(Range Accuracy), 방사 속도 정확도(Radial-Velocity Accuracy), 각도 불확실성, 감지 확률, RCS 또는 신호 강도 분포, 클러터, 오탐, 목표물 희소성(Target Sparsity)이 포함된다. 특히 동적 시나리오는 시뮬레이션의 도플러 특성이 실제 관측값과 일치하는지 평가하는 데 유용하다. 후속 추적 및 센서 융합 성능도 시스템 수준의 추가적인 검증 기준을 제공한다.

실용적인 레이더 시뮬레이션 아키텍처(Radar Simulation Architecture)는 환경 기하구조(Environment Geometry), 재질 특성(Material Characteristics), 레이더 구성(Radar Configuration), 전자기파 전파(Electromagnetic Propagation), 거리 추정, 도플러 처리(Doppler Processing), RCS 모델링, 노이즈, 클러터, 시간 특성, 감지 결과 생성을 연결한다. 필요한 충실도(Fidelity)는 알고리즘 개발, 합성 데이터 생성, 센서 융합 또는 상세 센서 검증 중 어떤 목적을 가지는지에 따라 결정해야 한다. 적절한 모델링을 통해 레이더 시뮬레이션은 다양한 로봇 운용 환경에서 강건한 인지(Robust Perception)와 시뮬레이션-현실 전이(Sim2Real) 개발을 지원할 수 있다.

## 04.05. IMU Simulation Noise Bias Drift Modeling [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

관성 측정 장치(Inertial Measurement Unit, IMU)는 일반적으로 3축 가속도계(Three-Axis Accelerometer)와 3축 자이로스코프(Three-Axis Gyroscope)를 이용하여 비력(Specific Force)과 각속도(Angular Velocity)를 측정함으로써 로봇의 움직임을 추정한다. 일부 장치는 자기계(Magnetometer)를 추가로 포함하지만 핵심 관성 모델(Inertial Model)은 가속도와 회전 속도 측정을 기반으로 한다. IMU 시뮬레이션(IMU Simulation)은 가상 로봇의 움직임으로부터 이러한 신호를 생성하고 실제 물리 센서의 특성을 근사하는 다양한 오차를 추가하여 센서 출력을 재현한다.

시뮬레이터는 IMU가 장착된 강체(Rigid Body)의 정답 운동 정보(Ground-Truth Motion)에서 시작한다. 선형 위치, 속도, 가속도, 자세(Orientation), 각속도는 물리 엔진(Physics Engine)에서 얻을 수 있으며 IMU 좌표계(IMU Coordinate Frame)로 변환할 수 있다. 이러한 이상적인 물리량은 센서 오차, 샘플링 효과(Sampling Effect), 노이즈가 적용되기 전에 시뮬레이션 가속도계와 자이로스코프 측정값을 생성하기 위한 기준값을 제공한다.

가속도계(Accelerometer)는 일반적인 월드 좌표계(World Frame)의 선형 가속도를 직접 측정하지 않는다. 가속도계는 센서 좌표계에서 표현되는 비력(Specific Force), 즉 중력을 제외한 가속 효과를 측정한다. 따라서 수평면에서 정지한 IMU도 일반적으로 0이 아니라 중력 가속도에 가까운 크기의 값을 출력한다. 그러므로 중력(Gravity)의 올바른 처리와 좌표 변환(Coordinate Transformation)은 물리적으로 일관된 가속도계 시뮬레이션을 구현하기 위한 기본 요소이다.

자이로스코프(Gyroscope)는 센서의 세 축을 중심으로 하는 각속도를 측정한다. 이상적인 자이로 측정값은 시뮬레이션된 강체의 각속도를 센서 장착 방향에 따라 IMU 좌표계로 변환하여 얻을 수 있다. 회전 운동 중 이러한 측정값은 플랫폼이 각 축을 중심으로 얼마나 빠르게 회전하는지를 나타낸다. 이후 현실적인 시뮬레이션에서는 이상적인 각속도에 편향(Bias), 랜덤 노이즈(Random Noise), 스케일 팩터 오차(Scale-Factor Error), 드리프트(Drift), 양자화(Quantization) 등의 센서 불완전성을 적용한다.

센서 출력은 이상적인 신호에 여러 오차 성분을 더하는 형태의 측정 모델(Measurement Model)로 표현할 수 있다. 각각의 가속도계 또는 자이로스코프 축에서 측정값은 일정한 편향(Constant Bias), 시간에 따라 변화하는 편향(Time-Varying Bias), 랜덤 측정 노이즈(Random Measurement Noise), 스케일 팩터 오차, 축 정렬 오차(Axis Misalignment), 양자화를 포함할 수 있다. 이러한 요소는 서로 다른 물리적 원인에서 발생하고 단기 및 장기 내비게이션 알고리즘에 서로 다른 영향을 주므로 개별적으로 구분하여 모델링하는 것이 유용하다.

백색 잡음(White Noise)은 연속된 측정값에서 빠르게 변화하는 무작위 불확실성을 나타낸다. 일반적으로 영평균 가우시안 노이즈(Zero-Mean Gaussian Noise)를 이용하여 근사하며 가속도계와 자이로스코프 채널에 독립적으로 적용한다. 적절한 노이즈 크기는 센서 품질, 샘플링 주파수(Sampling Frequency), 대역폭(Bandwidth)에 따라 달라진다. 백색 잡음은 충분히 긴 시간 동안 평균하면 평균값이 0에 가까워지지만 적분 과정에서는 그 영향이 속도, 자세, 위치 추정값에 누적되므로 현실적인 노이즈 모델링이 중요하다.

편향(Bias)은 실제 센서 신호에 추가되는 오프셋(Offset)이다. 작은 일정 편향을 가진 자이로스코프는 센서가 정지해 있어도 회전이 발생하는 것처럼 측정하며, 가속도계 편향은 측정된 비력에 지속적인 오차를 발생시킨다. 이러한 작은 오프셋도 관성항법(Inertial Navigation)에서는 센서 측정값을 반복적으로 적분하기 때문에 중요하다. 자이로스코프 편향은 자세 오차를 증가시키며 잘못된 자세 추정은 이후 중력이 병진 운동(Translational Motion)에 잘못 투영되는 원인이 된다.

편향은 실제 동작 과정에서 완전히 일정하게 유지되지 않는다. 온도 변화, 전자회로, 기계적 응력(Mechanical Stress), 노화(Aging), 센서 내부 특성에 따라 오프셋이 시간에 따라 천천히 변화할 수 있다. 이러한 현상은 일반적으로 편향 드리프트(Bias Drift) 또는 편향 불안정성(Bias Instability)으로 표현한다. 시뮬레이터에서는 랜덤 워크(Random Walk) 또는 1차 상관 모델(First-Order Correlated Model)과 같은 확률 과정(Stochastic Process)을 이용하여 서서히 변화하는 편향을 모델링할 수 있으며, 이를 통해 일정한 오프셋만 적용하는 것보다 현실적인 장시간 동작 특성을 구현할 수 있다.

랜덤 워크 모델(Random Walk Model)은 시간이 지나면서 점진적으로 변화하는 오차를 표현하는 데 특히 유용하다. 각 샘플마다 완전히 독립적인 편향을 생성하는 대신 이전 편향 상태에 작은 무작위 값을 추가한다. 이렇게 생성된 오차는 시간적 상관관계(Temporal Correlation)를 가지며 초기값에서 서서히 이동한다. 자이로스코프 및 가속도계 편향 랜덤 워크는 지속적으로 변화하는 관성 센서 편향을 추정하고 보상해야 하는 상태 추정기(State Estimator)를 시험할 때 중요하다.

노이즈 밀도(Noise Density)는 IMU 사양에서 자주 제공되며 대역폭에 대한 측정 노이즈 특성을 나타낸다. 이 값을 이산 시뮬레이션 노이즈(Discrete Simulation Noise)로 변환하려면 샘플링 간격(Sampling Interval) 또는 샘플링 주파수를 고려해야 한다. 모든 시뮬레이션 갱신 주파수에서 동일한 표준편차를 단순하게 적용하면 일관되지 않은 센서 특성이 발생할 수 있다. 따라서 물리적으로 의미 있는 시뮬레이터는 연속시간 노이즈 파라미터(Continuous-Time Noise Parameter)를 로봇 소프트웨어에 전달되는 이산시간 측정값(Discrete-Time Measurement)과 연결해야 한다.

스케일 팩터 오차(Scale-Factor Error)는 실제 물리 입력과 측정 출력 사이에 발생하는 비례 오차를 의미한다. 가속도계의 스케일 팩터가 공칭값(Nominal Value)보다 약간 높으면 실제 가속도가 증가할수록 측정 가속도 역시 비례하여 더 크게 나타난다. 자이로스코프에서도 동일한 특성이 발생할 수 있다. 스케일 팩터 오차는 각 축에 곱셈 계수(Multiplicative Coefficient)를 적용하여 표현할 수 있으며 제조 공차(Manufacturing Tolerance) 또는 보정 불확실성(Calibration Uncertainty)을 재현하기 위해 무작위화할 수도 있다.

축 정렬 오차(Axis Misalignment)와 교차축 감도(Cross-Axis Sensitivity)는 실제 센싱 축이 완벽하게 직교하지 않거나 가정된 센서 좌표계와 정확하게 정렬되지 않을 때 발생한다. 이에 따라 한 축 방향의 움직임이 다른 측정 채널에도 영향을 줄 수 있다. 간결한 시뮬레이션 모델에서는 이상적인 가속도 또는 각속도 벡터에 보정 행렬(Calibration Matrix)을 적용하여 이러한 특성을 표현할 수 있다. 이러한 오차는 고정밀 위치 추정, 내비게이션, 센서 보정 연구에서 중요하게 작용한다.

양자화(Quantization)는 디지털 IMU가 유한한 수치 해상도(Numerical Resolution)를 사용하여 측정값을 표현하기 때문에 발생한다. 연속적인 가속도 및 각속도 값은 센서 측정 범위와 아날로그-디지털 변환(Analog-to-Digital Conversion)에 의해 결정되는 이산 출력 단계로 변환된다. 양자화는 큰 동적 움직임에서는 영향이 작을 수 있지만 작은 신호, 저가형 센서 또는 정밀 상태 추정에서는 중요한 영향을 줄 수 있다. 실제 움직임이 설정된 측정 범위를 초과하는 경우 발생하는 포화(Saturation) 역시 모델링해야 한다.

IMU 샘플링(IMU Sampling)은 연속적이지 않고 이산적으로 수행된다. 가속도계와 자이로스코프는 장치에 따라 초당 수십 회에서 수백 또는 수천 회의 샘플링 주파수로 동작할 수 있다. 시뮬레이션은 물리 엔진의 모든 상태 갱신값을 자동으로 제공하는 대신 목표 센서의 갱신 주기(Update Rate)에 맞추어 측정값을 생성해야 한다. 물리 엔진 주파수와 센서 주파수의 차이를 고려하지 않으면 내비게이션 소프트웨어가 실제보다 높은 시간 해상도를 사용할 수 있다는 비현실적인 가정이 발생할 수 있다.

타임스탬프 동작(Timestamp Behavior)은 IMU 데이터를 카메라, 라이다, 레이더, 위성항법시스템(GNSS), 휠 오도메트리(Wheel Odometry), 관절 엔코더(Joint Encoder)와 융합할 때 중요하다. 센서 지연시간(Sensor Latency), 타임스탬프 오프셋(Timestamp Offset), 통신 지연(Communication Delay), 지터(Jitter)는 서로 다른 물리적 시점의 측정값이 함께 처리되는 원인이 될 수 있다. IMU는 일반적으로 다른 센서보다 훨씬 높은 주파수로 동작하므로 작은 동기화 오차도 모션 보상(Motion Compensation), 시각-관성 오도메트리(Visual-Inertial Odometry), 라이다 디스큐잉(LiDAR Deskewing), 다중 센서 상태 추정(Multi-Sensor State Estimation)에 영향을 줄 수 있다.

IMU의 장착 자세(Mounting Pose)는 로봇 몸체 좌표계(Robot Body Frame)에 대한 센서의 위치와 회전을 정의한다. 방향 오차는 측정된 가속도와 각속도를 잘못된 방향으로 직접 회전시킨다. 로봇의 회전 중심으로부터 센서까지의 병진 거리(Translation)도 중요한데, 회전 운동이 센서 위치에서 접선 가속도(Tangential Acceleration)와 구심 가속도(Centripetal Acceleration)를 발생시킬 수 있기 때문이다. 따라서 고충실도 시뮬레이션에서는 로봇 원점만 사용하는 대신 실제 가상 장착 위치에서 관성 측정값을 계산해야 한다.

진동(Vibration)은 관성 측정을 교란하는 또 다른 중요한 요인이다. 모터, 기어박스, 휠, 프로펠러, 서스펜션 시스템(Suspension System), 구조적 공진(Structural Resonance)은 진동성 가속도와 각속도 성분을 생성할 수 있다. 이러한 교란은 단순한 가우시안 백색 잡음만으로 충분히 표현되지 않을 수 있다. 시뮬레이션에서는 주파수 의존적 진동 신호(Frequency-Dependent Vibration Signal)를 추가하거나 충분히 상세한 강체 동역학(Rigid-Body Dynamics)으로부터 자연스럽게 발생하도록 하여 필터링 및 상태 추정기의 강건성(Robustness)을 평가할 수 있다.

장시간 또는 고정밀 동작이 중요한 경우 온도 의존성(Temperature Dependence)을 모델링할 수 있다. 가속도계와 자이로스코프의 편향, 스케일 팩터, 노이즈 특성은 센서 온도에 따라 변화할 수 있다. 상세한 시뮬레이터에서는 온도 의존적 보정 파라미터와 동작 과정에서 변화하는 열 상태(Thermal State)를 정의할 수 있다. 단순한 시뮬레이션에서는 명시적인 열역학 모델 없이 실행마다 편향을 무작위화하여 장치별 편차와 운용 조건의 차이를 근사할 수 있다.

IMU 오차는 내비게이션 알고리즘이 측정값을 시간에 따라 적분하기 때문에 특히 중요하다. 각속도는 자세를 추정하기 위해 적분되며, 가속도는 추정된 자세를 이용하여 좌표 변환한 후 속도와 위치를 계산하기 위해 다시 적분된다. 따라서 노이즈, 편향, 보정 오차는 측정 수준에 제한되지 않고 시간이 지나면서 누적된다. 추가적인 관측값을 이용해 추정 상태를 제한하지 않는 한 순수 관성항법(Pure Inertial Navigation)은 필연적으로 드리프트를 발생시킨다.

센서 융합(Sensor Fusion)은 상호 보완적인 측정값을 결합하여 이러한 드리프트를 감소시킨다. 위성항법시스템(GNSS)은 실외에서 전역 위치(Global Position)를 제공할 수 있고, 카메라와 라이다는 환경 특징을 기준으로 움직임을 제한할 수 있으며, 휠 오도메트리는 평면 이동을 추정할 수 있다. 다른 센서 역시 방향이나 구조적 정보를 제공할 수 있다. 따라서 확장 칼만 필터(Extended Kalman Filter), 팩터 그래프(Factor Graph), 시각-관성 시스템(Visual-Inertial System) 등의 상태 추정기가 실제 물리 센서와 유사한 문제를 해결하도록 시뮬레이션 IMU 데이터에 현실적인 불완전성을 포함해야 한다.

시뮬레이션에서는 가상 IMU에 현실적인 오차를 추가하더라도 정확한 정답 정보(Ground Truth)를 사용할 수 있다. 정확한 자세, 속도, 가속도, 각속도, 생성된 편향 상태(Bias State)를 노이즈가 포함된 측정값과 함께 기록할 수 있다. 이를 통해 개발자는 불확실한 실제 기준 측정값에만 의존하지 않고 센서 노이즈, 잘못된 보정, 동기화 문제, 좌표 변환 오류 또는 상태 추정기 설계에서 발생하는 오차를 구분할 수 있으므로 상태 추정 알고리즘을 디버깅하는 데 매우 유용하다.

IMU 노이즈 모델의 파라미터는 가능한 경우 실제 센서 측정값 또는 제조업체 사양(Manufacturer Specification)으로부터 도출해야 한다. 정적 데이터(Static Data)를 이용하면 편향과 단기 노이즈를 확인할 수 있으며 장시간 기록을 통해 드리프트와 시간적으로 상관된 특성을 분석할 수 있다. 앨런 편차 분석(Allan Deviation Analysis)과 같은 기법은 서로 다른 시간 척도에서 관성 센서의 노이즈 과정을 특성화하는 데 일반적으로 사용된다. 이렇게 얻은 파라미터를 시뮬레이터에 적용하면 목표 하드웨어와의 대응성을 향상시킬 수 있다.

도메인 랜덤화(Domain Randomization)는 시뮬레이션 에피소드마다 가속도계 노이즈, 자이로스코프 노이즈, 초기 편향(Initial Bias), 편향 드리프트, 스케일 팩터, 축 정렬, 장착 자세, 지연시간, 샘플링 주파수, 진동을 변화시킬 수 있다. 이를 통해 학습 또는 상태 추정 시스템이 완벽하게 보정된 하나의 가상 IMU에 의존하는 것을 방지할 수 있다. 적절한 파라미터 변화는 제조 공차, 노화, 보정 불확실성, 운용 조건을 표현하면서 알고리즘을 실제 로봇으로 전이할 때 강건성을 향상시킬 수 있다.

검증(Validation)은 통제된 정적 및 동적 운동 조건에서 시뮬레이션 IMU와 실제 IMU의 측정값을 비교하는 방식으로 수행해야 한다. 주요 비교 대상에는 평균 편향(Mean Bias), 표준편차(Standard Deviation), 주파수 특성(Spectral Characteristics), 시간에 따른 드리프트, 포화 동작, 샘플 사이의 상관관계가 포함된다. 시스템 수준에서는 동일한 상태 추정 알고리즘에 유사한 궤적의 시뮬레이션 데이터와 실제 센서 데이터를 각각 입력하여 발생하는 자세, 속도, 위치 오차를 비교함으로써 추가적인 검증을 수행할 수 있다.

실용적인 IMU 시뮬레이션 아키텍처(IMU Simulation Architecture)는 강체 정답 정보(Rigid-Body Ground Truth), 중력, 좌표 변환, 가속도계 및 자이로스코프 모델, 확률적 노이즈(Stochastic Noise), 편향 변화(Bias Evolution), 보정 오차(Calibration Error), 샘플링, 시간 특성, 장착 기하구조(Mounting Geometry)를 연결한다. 목표는 완벽한 운동 데이터에 단순히 무작위 가우시안 값을 추가하는 것이 아니라 시간이 지나면서 상태 추정에 영향을 미치는 실제 오차 메커니즘을 재현하는 것이다. 적절한 모델링을 통해 가상 IMU는 내비게이션 개발, 센서 융합, 검증, 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 효과적인 도구가 된다.

## 04.06. Force Torque Sensor Simulation Contact Forces [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

힘-토크 센서 시뮬레이션(Force-Torque Sensor Simulation)은 로봇과 환경 사이에서 발생하는 기계적 상호작용 하중(Mechanical Interaction Load)을 재현한다. 이러한 센서는 일반적으로 로봇의 손목, 관절, 그리퍼(Gripper), 발 또는 구조적 인터페이스(Structural Interface)에 장착되며 접촉 과정에서 발생하는 힘과 모멘트(Moment)에 대한 정보를 제공한다. 현실적인 시뮬레이션을 통해 조작(Manipulation), 조립(Assembly), 보행(Locomotion), 충돌 감지(Collision Detection), 순응 제어(Compliant Control) 알고리즘은 물리 엔진(Physics Engine)의 이상적인 접촉 정보에 직접 접근하는 대신 실제 센서와 유사한 측정값을 기반으로 동작할 수 있다.

6축 힘-토크 센서(Six-Axis Force-Torque Sensor)는 일반적으로 세 개의 병진 힘 성분(Translational Force Component)과 세 개의 회전 모멘트 성분(Rotational Moment Component)을 측정한다. 측정값은 정의된 센서 좌표계(Sensor Coordinate Frame)에서 Fx, Fy, Fz, Tx, Ty, Tz를 포함하는 렌치(Wrench)로 표현할 수 있다. 이러한 렌치를 시뮬레이션하려면 물리 엔진에서 관련 접촉력을 수집하고 힘과 모멘트를 가상 센서에 연결된 좌표계로 일관되게 변환해야 한다.

접촉력(Contact Force)은 시뮬레이션된 강체(Rigid Body) 사이의 상호작용에서 발생한다. 두 충돌 기하구조(Collision Geometry)가 물리 엔진의 접촉 모델에 따라 침투하거나 서로 접근하면 솔버(Solver)는 비현실적인 상호 침투를 방지하고 접촉 제약조건(Contact Constraint)을 만족시키는 힘을 생성한다. 이러한 힘은 객체 질량, 상대 운동, 충돌 기하구조, 재질 파라미터(Material Parameter), 솔버 설정, 적분 시간 간격(Integration Timestep)에 따라 달라진다. 따라서 힘 센서의 현실성은 기반이 되는 접촉 시뮬레이션의 충실도(Fidelity)와 밀접하게 연결된다.

수직 접촉력(Normal Contact Force)은 접촉 표면에 거의 수직으로 작용하며 객체가 서로 통과하는 것을 방지한다. 시뮬레이터에 따라 접촉은 강체 제약(Rigid Constraint), 순응성 스프링-댐퍼 모델(Compliant Spring-Damper Model), 페널티 방법(Penalty Method) 또는 이러한 방법의 조합으로 모델링할 수 있다. 접촉 강성(Contact Stiffness)과 감쇠(Contact Damping)는 최대 힘(Peak Force), 침투 깊이(Penetration Depth), 진동(Oscillation), 안정화 특성(Settling Behavior)에 큰 영향을 주므로 시뮬레이션 힘 측정값을 제어기 개발에 사용하는 경우 중요한 파라미터가 된다.

접선 접촉력(Tangential Contact Force)은 주로 상호작용하는 표면 사이의 마찰(Friction)에서 발생한다. 정지 마찰(Static Friction)은 접선 방향 힘이 한계를 초과할 때까지 상대적인 미끄러짐을 방지할 수 있으며, 동적 마찰(Dynamic Friction)은 미끄러짐이 발생하는 동안 작용한다. 물리 엔진은 일반적으로 쿨롱 마찰(Coulomb Friction) 또는 이와 유사한 모델을 이용하여 이러한 특성을 근사한다. 파지 안정성(Grasp Stability), 밀기(Pushing), 삽입(Insertion), 보행, 휠 견인력(Wheel Traction) 등 다양한 로봇 동작이 접선 접촉력에 직접 의존하므로 정확한 마찰 파라미터가 중요하다.

센서 원점(Sensor Origin)에서 떨어진 위치에서 접촉이 발생하면 힘뿐만 아니라 토크(Torque)도 생성된다. 발생하는 모멘트는 센서 기준점과 접촉 위치 사이의 모멘트 암(Lever Arm) 및 적용된 힘에 따라 결정된다. 여러 접촉이 동시에 발생하면 각각의 힘과 모멘트를 합산하여 순 렌치(Net Wrench)를 계산해야 한다. 이러한 원리는 로봇 조작 및 조립 작업에서 사용하는 손목 힘-토크 센서(Wrist Force-Torque Sensor)의 동작을 재현하는 데 필수적이다.

좌표계 처리(Coordinate-Frame Handling)는 매우 중요하다. 물리 엔진은 월드 좌표계(World Frame), 몸체 좌표계(Body Frame), 접촉 좌표계(Contact Frame)에서 접촉력을 계산할 수 있지만 로봇 소프트웨어는 센서 좌표계의 측정값을 요구할 수 있다. 따라서 시뮬레이터는 가상 측정값을 출력하기 전에 힘 벡터를 올바르게 회전시키고 모멘트를 정확하게 변환해야 한다. 잘못된 좌표 변환은 수치상으로는 그럴듯하지만 물리적으로 잘못된 방향의 값을 생성할 수 있으므로 좌표계 규약(Coordinate Convention)과 센서 장착 변환(Sensor Mounting Transformation)은 검증의 중요한 요소이다.

로봇이 외부 객체와 접촉하지 않는 경우에도 중력(Gravity)과 관성 하중(Inertial Load)이 힘-토크 측정값에 나타날 수 있다. 그리퍼와 페이로드(Payload)를 지지하는 손목 장착 센서는 이들의 무게와 관련된 하중을 측정하며, 가속 및 회전 운동은 추가적인 관성력과 모멘트를 발생시킨다. 특히 알고리즘이 중력 보상(Gravity Compensation), 페이로드 추정(Payload Estimation), 충돌 감지를 수행하는 경우 시뮬레이션에서는 필요에 따라 외부 접촉 하중과 전체 센서 렌치(Complete Sensor Wrench)를 구분할 수 있어야 한다.

힘-토크 센서는 유한한 측정 범위(Measurement Range)를 가진다. 이 한계를 초과하는 하중은 수치적으로 계속 증가하는 대신 포화(Saturation)되거나 클리핑(Clipping)될 수 있다. 해상도(Resolution)와 양자화(Quantization) 역시 표현할 수 있는 최소 변화량을 제한한다. 이러한 특성을 모델링하면 시뮬레이션 제어기가 비현실적으로 정밀한 측정값에 의존하는 것을 방지할 수 있으며 섬세한 조작(Delicate Manipulation), 작은 접촉력, 정밀 조립(Precision Assembly), 저가형 센싱 하드웨어를 다루는 경우 특히 중요하다.

현실적인 센서 동작이 필요한 경우 이상적인 렌치 값에 측정 노이즈(Measurement Noise)를 추가해야 한다. 각 힘 및 토크 축은 서로 다른 노이즈 특성을 가질 수 있으며 노이즈 크기는 센서 범위, 전자회로, 필터링(Filtering), 샘플링 주파수(Sampling Frequency)에 따라 달라질 수 있다. 가우시안 백색 잡음(Gaussian White Noise)은 기본적인 모델로 사용할 수 있지만 실제 센서에는 시간적으로 상관된 교란(Correlated Disturbance), 진동 성분, 전기적 간섭(Electrical Interference), 하중 의존적 측정 불확실성(Load-Dependent Measurement Uncertainty)이 포함될 수 있다.

편향(Bias)은 측정된 힘이나 토크에 지속적인 오프셋(Offset)을 발생시킨다. 따라서 실제 외부 렌치가 0인 경우에도 센서가 작은 하중을 출력할 수 있다. 편향은 전자회로, 장착 응력(Mounting Stress), 온도, 페이로드 변화, 불완전한 보정(Imperfect Calibration) 등에서 발생할 수 있다. 시뮬레이션 편향은 한 번의 실행 동안 일정하게 유지하거나 시간에 따라 천천히 변화하도록 설정할 수 있다. 특히 상대적으로 작은 힘에 반응하는 접촉 임계값(Contact Threshold)이나 제어기를 평가할 때 편향을 포함하는 것이 중요하다.

교차축 결합(Cross-Axis Coupling)은 한 센서 축에 적용된 하중이 다른 축의 측정값에 영향을 미치는 현상이다. 실제 다축 힘-토크 센서(Multi-Axis Force-Torque Sensor)는 기계적 구조와 변형률 측정(Strain Measurement)을 이용하므로 각 측정 채널이 완전히 독립적이지 않을 수 있다. 이러한 상호작용을 보상하기 위해 일반적으로 보정 행렬(Calibration Matrix)을 사용한다. 시뮬레이션에서는 이상적인 렌치에 행렬 변환을 적용하여 잔여 결합(Residual Coupling)을 표현할 수 있으며 이를 통해 정밀 조작 및 힘 제어 알고리즘을 더욱 현실적으로 시험할 수 있다.

샘플링(Sampling)과 필터링은 힘 측정값의 동적 특성(Dynamic Characteristics)에 영향을 준다. 물리 엔진은 초당 수백 또는 수천 단계로 갱신될 수 있지만 실제 힘-토크 센서는 이와 다른 샘플링 주파수로 동작하고 내부 필터링(Internal Filtering)을 적용할 수 있다. 따라서 모든 솔버 반복(Solver Iteration)에서 원시 물리 힘을 그대로 출력하면 비현실적인 관측값이 생성될 수 있다. 센서 모델은 로봇 소프트웨어가 예상하는 갱신 주기(Update Rate), 필터링 대역폭(Filtering Bandwidth), 지연시간(Latency), 타임스탬프 동작(Timestamp Behavior)을 재현해야 한다.

접촉 시뮬레이션(Contact Simulation)은 특히 갑작스러운 충돌이나 높은 접촉 강성과 상대적으로 큰 적분 시간 간격이 결합되는 경우 고주파 힘 스파이크(High-Frequency Force Spike)를 생성할 수 있다. 실제 기계 구조와 센서는 순응성(Compliance)과 대역폭 제한(Bandwidth Limitation)을 가지므로 이러한 과도 응답(Transient Response)을 변화시킨다. 따라서 시뮬레이션의 최대 힘을 실제 물리 측정값과 유사한 값으로 해석하려면 적절한 접촉 파라미터, 충분히 안정적인 시뮬레이션 시간 간격, 센서 필터링이 필요하다.

순응 접촉 모델(Compliant Contact Model)은 부드러운 손가락 끝(Soft Fingertip), 고무 표면(Rubber Surface), 변형 가능한 패드(Deformable Pad), 흡착 인터페이스(Suction Interface) 등 기계적으로 유연한 구조를 포함하는 작업에서 특히 유용하다. 스프링-댐퍼 근사(Spring-Damper Approximation)를 사용하면 침투 또는 변형과 수직 힘 사이의 관계를 표현하면서 감쇠를 통해 에너지를 소산할 수 있다. 더 정교한 시뮬레이션에서는 변형 가능한 재질을 직접 모델링할 수 있지만 단순화된 순응성 모델도 훨씬 낮은 계산 비용으로 제어기 개발에 충분한 동작을 제공할 수 있다.

조작 작업(Manipulation Task)은 힘 시뮬레이션에 높은 수준의 정확성을 요구한다. 파지 과정에서는 접촉력이 객체의 안정적인 유지 또는 미끄러짐(Slip)을 결정한다. 삽입 작업에서는 작은 측면 힘과 토크를 이용하여 정렬 오차(Alignment Error)를 감지할 수 있으며, 표면 추종(Surface Following)에서는 움직임을 유지하면서 수직 힘을 제어한다. 현실적인 시뮬레이션 렌치 측정값을 통해 임피던스 제어(Impedance Control), 어드미턴스 제어(Admittance Control), 하이브리드 위치-힘 제어(Hybrid Position-Force Control), 학습 기반 조작 정책(Learned Manipulation Policy)을 실제 하드웨어 배치 전에 개발할 수 있다.

힘-토크 센싱(Force-Torque Sensing)은 보행 로봇(Legged Robot)과 휴머노이드(Humanoid)에서도 중요하다. 발이나 발목에 위치한 센서는 하중 분포와 접촉 안정성을 나타내는 지면 반력(Ground Reaction Force)과 모멘트를 추정할 수 있다. 시뮬레이션된 접촉력은 균형 제어(Balance Control), 보행 패턴 개발(Gait Development), 압력 중심 추정(Center-of-Pressure Estimation), 접촉 상태 감지(Contact-State Detection)를 지원한다. 이러한 측정의 정확도는 지형 기하구조, 마찰, 접촉 강성, 시간 간격 선택, 물리 솔버에 크게 의존한다.

모바일 로봇(Mobile Robot)은 휠과 지면의 접촉을 통해 힘 정보를 간접적으로 활용할 수 있다. 수직 하중은 사용 가능한 견인력(Traction)에 영향을 주며 종방향 및 횡방향 힘은 가속, 제동, 선회 동작을 결정한다. 모바일 플랫폼의 모든 휠에 전용 힘-토크 센서가 장착되어 있지 않더라도 시뮬레이션된 접촉력은 서스펜션 동작(Suspension Behavior), 지형 상호작용(Terrain Interaction), 견인력 한계(Traction Limit), 휠 슬립(Wheel Slip), 페이로드 변화에 따른 제어 성능을 분석하는 데 유용하다.

충돌 감지(Collision Detection)에서도 힘 또는 토크 임계값을 사용할 수 있다. 로봇과 장애물 사이의 예상하지 못한 접촉은 보호 동작(Protective Behavior)을 실행할 수 있는 기계적 하중을 발생시킨다. 시뮬레이션 센서에 노이즈가 전혀 없다면 실제 환경에서 사용하기에는 지나치게 민감한 임계값이 선택될 수 있다. 편향, 노이즈, 진동, 지연, 과도 접촉 효과를 추가하면 실제 시험 전에 더욱 현실적인 측정 조건에서 안전 로직(Safety Logic)을 평가할 수 있다.

여러 접촉이 동시에 발생하는 경우 각각의 힘을 신중하게 집계(Aggregation)해야 한다. 그리퍼는 여러 지점에서 객체와 접촉할 수 있고 로봇의 발은 유한한 영역에 걸쳐 불규칙한 지형과 접촉할 수 있으며 관절형 메커니즘(Articulated Mechanism)은 동시에 여러 충돌력을 받을 수 있다. 시뮬레이터는 어떤 접촉이 가상 센서의 기계적 측정에 기여하는지를 결정하고 변환된 각각의 렌치를 합산해야 한다. 접촉을 잘못 포함하거나 제외하면 최종 센서 출력이 크게 왜곡될 수 있다.

정답 접촉 정보(Ground-Truth Contact Information)는 시뮬레이션이 제공하는 중요한 장점이다. 정확한 접촉 위치(Contact Location), 표면 법선(Surface Normal), 침투량(Penetration Measure), 상대 속도(Relative Velocity), 객체 식별자(Object Identity), 솔버가 생성한 힘을 노이즈가 포함된 힘-토크 측정값과 함께 기록할 수 있다. 이를 통해 개발자는 제어기가 특정 하중에 반응한 이유를 분석하고 물리적 접촉 동작과 센서 모델 오류, 필터링 효과, 좌표 변환 문제 또는 제어 시스템 불안정성을 구분할 수 있다.

합성 접촉 데이터(Synthetic Contact Data)는 파지 평가(Grasp Evaluation), 접촉 상태 분류(Contact-State Classification), 조작 정책 학습(Manipulation Policy Learning), 미끄러짐 예측(Slip Prediction), 이상 탐지(Anomaly Detection) 등의 머신러닝 응용을 지원할 수 있다. 힘-토크 측정값을 카메라, 깊이, 촉각(Tactile), 관절, 로봇 상태 정보와 동기화할 수 있으며 정확한 작업 라벨(Task Label)은 시뮬레이션에서 직접 얻을 수 있다. 이를 통해 실제 실험에서 상당한 계측 장비와 수작업 주석이 필요한 다중 모달 데이터셋(Multimodal Dataset)을 생성할 수 있다.

도메인 랜덤화(Domain Randomization)는 시뮬레이션 에피소드마다 마찰 계수(Friction Coefficient), 접촉 강성, 감쇠, 객체 질량, 페이로드, 센서 편향, 노이즈, 보정값, 장착 자세(Mounting Pose), 지연시간, 표면 특성을 변화시킬 수 있다. 이러한 변화는 제어 또는 학습 알고리즘이 하나의 이상적인 접촉 구성에 의존하는 것을 방지한다. 적절하게 설정된 범위는 기계적 공차(Mechanical Tolerance)와 환경 불확실성(Environmental Uncertainty)을 표현하면서 정책과 상태 추정기를 실제 로봇으로 전이할 때 강건성(Robustness)을 향상시킬 수 있다.

검증(Validation)은 대표적인 정적 및 동적 하중 조건에서 시뮬레이션 센서와 실제 센서 출력을 비교하는 방식으로 수행해야 한다. 유용한 시험에는 알려진 힘과 모멘트의 적용, 페이로드 하중, 충격(Impact), 미끄럼 접촉(Sliding Contact), 파지, 제어된 표면 상호작용 등이 포함된다. 비교 항목에는 정상 상태 오차(Steady-State Error), 노이즈, 과도 응답, 교차축 특성, 포화, 주파수 특성(Frequency Characteristics)이 포함될 수 있으며 후속 힘 제어 성능(Force-Control Performance)은 추가적인 시스템 수준 검증 지표가 된다.

실용적인 힘-토크 시뮬레이션 아키텍처(Force-Torque Simulation Architecture)는 강체 동역학(Rigid-Body Dynamics), 충돌 감지, 접촉 솔빙(Contact Solving), 마찰, 접촉 기하구조(Contact Geometry), 렌치 변환(Wrench Transformation), 센서 동역학(Sensor Dynamics), 노이즈, 편향, 샘플링, 필터링을 연결한다. 모든 영역에서 접촉 모델의 복잡성을 최대화하기보다 목표 상호작용 작업에 따라 필요한 충실도를 선택해야 한다. 적절한 모델링은 조작, 보행, 힘 제어, 안전 검증(Safety Validation), 합성 데이터 생성, 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 현실적인 기계적 피드백(Mechanical Feedback)을 제공한다.

## 04.07. Tactile Sensor Simulation for Robot Hand [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

촉각 센서 시뮬레이션(Tactile Sensor Simulation)은 로봇의 손가락과 손이 객체를 접촉하고, 파지하고, 누르고, 미끄러뜨리거나 조작할 때 감지하는 분산 접촉 정보(Distributed Contact Information)를 재현한다. 하나의 구조적 위치에서 전체 기계적 렌치(Net Mechanical Wrench)를 측정하는 손목 힘-토크 센서(Wrist Force-Torque Sensor)와 달리 촉각 센싱(Tactile Sensing)은 접촉 표면 전체에 걸쳐 공간적으로 국소화된 정보를 제공한다. 이를 통해 로봇은 접촉 위치, 압력 분포, 객체의 미끄러짐 발생 여부를 판단할 수 있다.

로봇 촉각 센서(Robotic Tactile Sensor)는 저항식(Resistive), 정전용량식(Capacitive), 압저항식(Piezoresistive), 압전식(Piezoelectric), 광학식(Optical), 자기식(Magnetic), 엘라스토머 기반(Elastomer-Based) 센싱 기술을 이용하여 구현할 수 있다. 물리적 원리는 서로 다르지만 시뮬레이션에서는 일반적으로 수직 압력(Normal Pressure), 접선력(Tangential Force), 변형(Deformation), 접촉 위치(Contact Location), 택셀 강도(Taxel Intensity) 등의 측정 가능한 물리량으로 추상화한다. 필요한 추상화 수준(Abstraction Level)은 제어 개발, 인지 연구, 합성 데이터 생성, 상세 센서 설계 중 어떤 목적을 가지는지에 따라 결정된다.

촉각 배열(Tactile Array)은 센싱 표면을 일반적으로 택셀(Taxel)이라고 부르는 개별 센싱 요소로 분할한다. 각각의 택셀은 작은 공간 영역을 나타내며 국소적인 접촉 상태와 관련된 값을 생성한다. 시뮬레이션된 손가락 끝(Fingertip)은 필요한 공간 해상도(Spatial Resolution)에 따라 수십 개에서 수백 또는 수천 개의 택셀을 포함할 수 있다. 높은 해상도는 상세한 접촉 기하구조(Contact Geometry)를 표현할 수 있지만 충돌 처리, 변형 모델링, 데이터 생성, 통신에 필요한 계산량도 증가시킨다.

촉각 시뮬레이션의 기본 입력은 로봇 손과 객체 사이의 접촉(Contact)이다. 물리 엔진(Physics Engine)은 접촉 위치, 표면 법선(Surface Normal), 침투 또는 변형, 상대 속도(Relative Velocity), 상호작용 힘(Interaction Force)을 결정한다. 이후 이러한 물리량을 촉각 센싱 표면에 투영할 수 있다. 센서 모델은 원시 접촉점(Raw Contact Point)을 직접 제공하는 대신 물리적 상호작용을 실제 촉각 장치에서 생성되는 것과 유사한 공간적 측정값(Spatial Measurement)으로 변환한다.

수직 압력(Normal Pressure)은 가장 중요한 촉각 물리량 중 하나이다. 객체가 손가락 끝을 누르면 접촉력은 이상적인 하나의 수학적 점에 집중되지 않고 일정한 영역에 분포한다. 시뮬레이터는 접촉 패치(Contact Patch), 커널 함수(Kernel Function), 변형 모델(Deformation Model)을 이용하여 수직력을 주변 택셀에 분배할 수 있다. 이렇게 생성된 압력 맵(Pressure Map)은 접촉 위치, 접촉 면적, 힘 분포, 객체 기하구조에 대한 정보를 제공한다.

접선력(Tangential Force)은 마찰(Friction)과 미끄러짐 발생 가능성(Impending Slip)에 대한 정보를 제공한다. 파지된 객체가 손가락 끝을 기준으로 움직이기 시작하면 접촉 영역 전체에서 전단 응력(Shear Stress)이 발생한다. 시뮬레이션 촉각 센서는 이러한 접선 성분을 수직 압력과 분리하여 표현할 수 있다. 수직 및 전단 정보를 결합하면 알고리즘이 파지 안정성(Grasp Stability)을 추정하고, 파지력을 조절하며, 미끄러짐을 감지하고, 객체를 놓치기 전에 대응할 수 있다.

접촉 패치 모델링(Contact-Patch Modeling)은 물리적 접촉이 가상 센서 표면에 어떻게 분포하는지를 결정한다. 강체 접촉 솔버(Rigid Contact Solver)는 소수의 이산 접촉점만 생성할 수 있지만 실제 부드러운 손가락 끝은 유한한 변형 영역을 형성한다. 시뮬레이션에서는 센서의 순응성(Compliance)에 따라 각각의 접촉점 주변으로 접촉력을 분산시키거나 변형 가능한 재질(Deformable Material)을 직접 모델링할 수 있다. 이러한 변환은 현실적인 촉각 이미지(Tactile Image)나 고밀도 압력 맵(Dense Pressure Map)을 생성하는 데 필수적이다.

부드러운 촉각 표면(Soft Tactile Surface)은 외부 객체와 센싱 요소 사이에 기계적 순응성(Mechanical Compliance)을 제공한다. 엘라스토머 층(Elastomer Layer)은 접촉 시 변형되므로 힘이 국소적으로 가해져도 주변 택셀이 함께 반응할 수 있다. 단순화된 모델에서는 스프링-댐퍼 시스템(Spring-Damper System) 또는 공간 평활화 커널(Spatial Smoothing Kernel)을 이용하여 이러한 동작을 근사할 수 있다. 고충실도 모델(High-Fidelity Model)은 유한요소법(Finite-Element Method)이나 변형체 시뮬레이션(Deformable-Body Simulation)을 사용할 수 있지만 계산 비용은 상당히 증가한다.

표면 기하구조(Surface Geometry)는 촉각 관측값에 큰 영향을 준다. 평면, 곡면, 모서리, 뾰족한 형상, 텍스처 표면은 동일한 전체 힘이 적용되더라도 서로 다른 접촉 패턴(Contact Pattern)을 생성한다. 시뮬레이션 센서의 해상도와 변형 모델이 충분하면 미세한 기하학적 특징도 고해상도 촉각 맵(High-Resolution Tactile Map)에 나타날 수 있다. 따라서 촉각 시뮬레이션은 객체 메시 품질(Object Mesh Quality), 충돌 기하구조, 손가락 끝 형상, 재질 순응성, 센서 공간 해상도를 서로 연결한다.

마찰(Friction)은 수직 하중과 사용 가능한 접선력 사이의 관계를 결정하므로 파지에서 핵심적인 요소이다. 객체와 손가락 끝의 재질이 다르면 서로 다른 마찰 계수(Coefficient of Friction)를 가지며 이에 따라 미끄러짐 특성도 달라질 수 있다. 촉각 시뮬레이터를 파지 제어기(Grasp Controller) 또는 미끄러짐 감지 알고리즘(Slip-Detection Algorithm) 개발에 사용하는 경우 마찰, 접촉 압력, 접선 운동, 전단 측정 사이의 상호작용을 유지해야 한다.

광학 촉각 센서(Optical Tactile Sensor)는 단순한 압력 배열과 다른 수준의 모델링이 필요하다. 이러한 센서는 일반적으로 카메라와 제어된 조명(Controlled Illumination)을 이용하여 내부 엘라스토머 표면의 변형을 관측한다. 따라서 시뮬레이션에서는 단순한 힘 값만 생성하는 대신 변형된 표면, 마커(Marker), 그림자, 표면 법선, 조명 변화를 렌더링해야 할 수 있다. 이렇게 생성된 합성 촉각 이미지(Synthetic Tactile Image)는 실제 센서에 적용하는 것과 동일한 컴퓨터 비전(Computer Vision) 또는 학습 파이프라인(Learning Pipeline)을 통해 처리할 수 있다.

촉각 데이터가 실제 하드웨어를 표현해야 하는 경우 노이즈(Noise)를 포함해야 한다. 각각의 택셀은 랜덤 측정 노이즈(Random Measurement Noise), 서로 다른 감도(Sensitivity), 데드존(Dead Zone), 오프셋(Offset), 시간적 변동(Temporal Fluctuation)을 가질 수 있다. 인접한 센싱 요소가 기계적 구조 또는 전자회로를 공유하기 때문에 공간적으로 상관된 노이즈(Spatially Correlated Noise)도 발생할 수 있다. 이러한 효과를 모델링하면 인지 및 제어 알고리즘이 비현실적으로 균일하고 완벽하게 반복 가능한 촉각 측정값에 의존하는 것을 방지할 수 있다.

편향(Bias)과 보정 편차(Calibration Variation)는 촉각 배열의 위치마다 다르게 나타날 수 있다. 접촉이 없는 상태에서도 특정 택셀의 기준값이 다른 택셀보다 약간 높게 나타날 수 있으며 동일한 압력에 대한 감도도 공간적으로 달라질 수 있다. 시뮬레이터는 보정 맵(Calibration Map)을 통해 택셀별 오프셋과 스케일 팩터(Scale Factor)를 설정할 수 있다. 가상 센서마다 이러한 파라미터를 무작위화하면 실제 로봇 손에서 발생하는 제조 편차(Manufacturing Variation)와 불완전한 보정도 재현할 수 있다.

촉각 센서는 유한한 공간 및 힘 해상도(Force Resolution)를 가진다. 매우 작은 접촉은 하나의 택셀에만 영향을 주거나 감지 임계값(Detection Threshold) 이하에 머물 수 있으며 큰 힘은 측정 범위를 초과하여 포화(Saturation)될 수 있다. 양자화(Quantization)는 사용할 수 있는 강도 수준을 추가로 제한한다. 알고리즘이 미세한 압력 변화를 이용하여 파지력을 조절하거나 작은 기하학적 특징을 식별하는 경우 해상도, 임계값, 양자화, 포화를 모델링하는 것이 중요하다.

시간적 동작(Temporal Behavior) 역시 촉각 측정에 영향을 준다. 실제 센서는 유한한 샘플링 주파수(Sampling Rate)로 동작하며 필터링(Filtering), 지연시간(Latency), 히스테리시스(Hysteresis), 기계적 이완(Mechanical Relaxation)을 포함할 수 있다. 부드러운 재질은 최초 접촉 이후에도 계속 변형될 수 있으며 하중이 제거된 이후 점진적으로 원래 상태로 복원될 수 있다. 현실적인 시뮬레이션은 이러한 특성을 포함하여 물리 엔진에서 접촉이 생성되거나 제거되는 순간 촉각 신호가 즉시 변화하는 대신 시간에 따라 변화하도록 할 수 있다.

미끄러짐 감지(Slip Detection)는 특히 세밀한 시간적 모델링이 필요하다. 초기 미끄러짐(Initial Slip)은 전단력 변화, 접촉 중심(Contact Centroid)의 이동, 진동, 촉각 표면의 압력 재분포로 나타날 수 있다. 이러한 신호를 시뮬레이션하면 큰 미끄러짐이 발생하기 전에 제어기가 파지력을 증가시키거나 손가락 움직임을 수정할 수 있다. 동적 마찰(Dynamic Friction), 센서 샘플링 주파수, 표면 텍스처(Surface Texture), 접촉 순응성은 최종적인 미끄러짐 특성(Slip Signature)에 모두 영향을 준다.

로봇 손(Robotic Hand)은 일반적으로 손가락 끝, 손가락 링크(Finger Link), 손바닥(Palm)에 분산된 여러 촉각 센싱 영역을 가진다. 각각의 센서는 손의 운동학적 체인(Kinematic Chain)에 대해 고유한 자세(Pose)와 로컬 좌표계(Local Coordinate System)를 가진다. 따라서 접촉 데이터는 올바른 센싱 표면에 연결되고 일관되게 좌표 변환되어야 한다. 다중 손가락 시뮬레이션(Multi-Finger Simulation)을 이용하면 전체 파지를 여러 공간적 접촉 관측값의 통합된 집합으로 표현할 수 있다.

촉각 센싱은 시각 정보(Visual Information)가 충분하지 않은 상황에서 특히 유용하다. 객체가 파지된 이후에는 객체의 일부가 손가락에 의해 가려질 수 있지만 접촉 측정값은 계속 직접적으로 사용할 수 있다. 촉각 데이터는 객체 자세(Object Pose)를 추정하고, 국소 기하구조(Local Geometry)를 인식하며, 접촉 상태 전이(Contact Transition)를 감지하고, 파지가 안정적인지 판단하는 데 활용할 수 있다. 시뮬레이션에서는 하드웨어 손상 위험이나 실제 객체에 반복적으로 계측 장비를 설치하지 않고도 이러한 동작을 연구할 수 있다.

손 내부 조작(In-Hand Manipulation)은 지속적으로 변화하는 촉각 패턴을 해석해야 한다. 손가락 사이에서 객체를 회전시키거나, 굴리거나, 위치를 변경하면 접촉 영역과 힘 분포가 센서 표면을 따라 이동한다. 시뮬레이션된 촉각 관측값은 안정적인 접촉을 유지하면서 손가락 움직임을 조정하는 제어기와 학습 정책(Learning Policy)을 지원할 수 있다. 이러한 작업은 접촉 역학(Contact Mechanics), 마찰, 센서 해상도, 시간적 충실도(Temporal Fidelity)를 종합적으로 평가하는 까다로운 시험 환경을 제공한다.

촉각 데이터는 관절 위치(Joint Position), 모터 토크(Motor Torque), 손목 힘-토크 센싱, 카메라, 깊이 센서(Depth Sensor)와 결합하여 다중 모달 인지(Multimodal Perception)를 구성할 수 있다. 비전(Vision)은 객체와 장면에 대한 전역 정보를 제공하고 촉각 센싱은 접촉 지점에서 국소적인 기계적 증거를 제공한다. 시뮬레이션에서는 공통 타임스탬프(Common Timestamp)와 정답 정보를 이용하여 이러한 모달리티를 동기화함으로써 시각-촉각 인지(Visuotactile Perception), 파지 추정, 접촉 인식 계획(Contact-Aware Planning), 조작 연구를 지원할 수 있다.

정답 접촉 정보(Ground-Truth Contact Information)는 촉각 시뮬레이션이 제공하는 주요 장점이다. 정확한 객체 식별자(Object Identity), 접촉 위치, 표면 법선, 힘, 상대 속도, 변형 상태(Deformation State), 미끄러짐 상태(Slip Condition)를 시뮬레이션 센서 출력과 함께 기록할 수 있다. 이러한 라벨은 실제 실험에서 정확하게 획득하기 어렵기 때문에 지도 학습(Supervised Learning), 디버깅(Debugging), 평가(Evaluation), 로봇 조작 실패에 대한 상세 분석에 활용할 수 있다.

합성 촉각 데이터셋(Synthetic Tactile Dataset)은 압력 맵, 전단 맵(Shear Map), 촉각 이미지, 접촉 마스크(Contact Mask), 객체 라벨, 파지 상태(Grasp State), 미끄러짐 라벨(Slip Label), 대응하는 로봇 상태 정보를 포함할 수 있다. 객체, 파지 자세, 힘, 움직임을 변화시키면서 많은 수의 상호작용 데이터를 자동으로 생성할 수 있다. 대규모 실제 접촉 데이터를 수집하고 라벨링하는 작업은 많은 노동력이 필요하고 센서 마모를 발생시킬 수 있으므로 이러한 방식은 데이터 기반 촉각 인지(Data-Driven Tactile Perception)에 특히 유용하다.

도메인 랜덤화(Domain Randomization)는 객체 기하구조, 마찰, 강성(Stiffness), 손가락 끝 순응성(Fingertip Compliance), 센서 해상도, 택셀 감도, 노이즈, 편향, 광학 센서의 조명, 장착 자세(Mounting Pose), 샘플링 주파수, 지연시간 등을 변화시킬 수 있다. 랜덤화를 통해 학습 알고리즘은 보다 다양한 촉각 형태와 기계적 상호작용을 경험할 수 있다. 적절하게 설정된 파라미터 분포(Parameter Distribution)는 센서 제조 차이에 대한 강건성(Robustness)을 향상시키고 이상적인 시뮬레이션 모델에 대한 의존도를 감소시킬 수 있다.

검증(Validation)은 동일한 접촉 조건에서 시뮬레이션 촉각 센서와 실제 촉각 센서를 비교하는 방식으로 수행해야 한다. 유용한 실험에는 제어된 압입(Controlled Indentation), 알려진 수직 및 접선 하중, 미끄러짐, 객체 모서리, 곡면, 반복 파지(Repeated Grasp)가 포함된다. 비교 지표로는 접촉 면적, 압력 분포, 힘 응답(Force Response), 공간 해상도, 노이즈, 히스테리시스, 미끄러짐 특성, 시간 응답(Temporal Response)을 사용할 수 있으며 후속 파지 또는 분류 성능도 함께 평가할 수 있다.

실용적인 촉각 시뮬레이션 아키텍처(Tactile Simulation Architecture)는 충돌 및 접촉 역학, 마찰, 표면 기하구조, 순응 변형(Compliant Deformation), 공간 센싱(Spatial Sensing), 센서 동역학(Sensor Dynamics), 노이즈, 보정, 시간적 동작을 연결한다. 필요한 충실도는 목표 촉각 기술과 조작 작업에 맞추어 결정해야 한다. 적절한 모델링을 통해 파지, 미끄러짐 감지, 손 내부 조작, 다중 모달 인지, 합성 데이터 생성, 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 현실적인 접촉 관측값을 제공할 수 있다.

## 04.08. GPS GNSS Simulation with Atmospheric Error [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

GPS 및 GNSS 시뮬레이션(GPS and GNSS Simulation)은 로봇, 자율주행차, 무인항공기(UAV), 기타 이동 시스템을 위한 위성 기반 위치 측정(Satellite-Based Positioning Measurement)을 재현한다. GPS는 보다 광범위한 전역 위성항법시스템(Global Navigation Satellite System, GNSS)을 구성하는 하나의 위성군(Constellation)이며, GNSS에는 갈릴레오(Galileo), 글로나스(GLONASS), 베이더우(BeiDou), 지역 위성항법시스템도 포함될 수 있다. 유용한 시뮬레이터는 로봇의 정답 운동 정보(Ground-Truth Motion)를 현실적인 항법 측정값으로 변환하면서 실제 수신기에서 발생하는 오차와 가용성 제한(Availability Limitation)을 재현한다.

GNSS 위치 결정(GNSS Positioning)은 위치와 시간 정보가 알려진 항법 위성(Navigation Satellite)에서 송신되는 신호를 기반으로 한다. 수신기는 여러 위성에서 도달하는 신호의 전파 시간(Propagation Time)을 추정하고 이러한 지연시간을 의사거리 측정값(Pseudorange Measurement)으로 변환한다. 수신기 시계가 위성 시계와 완벽하게 동기화되지 않기 때문에 일반적으로 3차원 위치와 수신기 시계 편향(Receiver Clock Bias)을 함께 추정하려면 최소 네 개의 독립적인 위성 측정값이 필요하다.

기본적인 시뮬레이션은 전역 좌표계(Global Coordinate System)로 표현된 로봇의 정답 위치(Ground-Truth Position)에서 시작할 수 있다. 응용 분야에 따라 시뮬레이터는 위도(Latitude), 경도(Longitude), 고도(Altitude), 지구 중심 지구 고정 좌표(Earth-Centered Earth-Fixed, ECEF), 또는 로컬 항법 좌표계(Local Navigation Frame)를 이용하여 상태를 표현할 수 있다. 로봇 소프트웨어는 전역 기준 GNSS 측정값을 로컬 기준의 IMU, 오도메트리(Odometry), 라이다(LiDAR), 지도 정보와 결합하는 경우가 많으므로 정확한 좌표 변환(Coordinate Transformation)이 중요하다.

이상적인 GNSS 시뮬레이터는 로봇의 정확한 위치를 그대로 출력할 수 있지만 이러한 측정값은 현실적인 내비게이션 개발에 적합하지 않다. 실제 수신기는 위성 기하구조(Satellite Geometry), 대기 전파(Atmospheric Propagation), 수신기 노이즈(Receiver Noise), 다중경로(Multipath), 신호 차단(Signal Blockage), 시계 오차(Clock Error), 위성 관련 불확실성의 영향을 받는다. 따라서 현실적인 시뮬레이션에서는 이상적인 기하학적 거리(Geometric Range)에 이러한 오차 요인을 적용하거나 적절한 센서 모델에 따라 최종 항법 해(Navigation Solution)를 직접 교란하여 측정값을 생성한다.

위성 기하구조(Satellite Geometry)는 위치 정확도(Positioning Accuracy)에 큰 영향을 미친다. 하늘의 여러 방향에 분산된 위성은 일반적으로 좁은 영역에 집중된 위성보다 우수한 기하학적 제약조건을 제공한다. 이러한 효과는 일반적으로 GDOP, PDOP, HDOP, VDOP와 같은 정밀도 저하율(Dilution of Precision, DOP) 지표로 표현한다. 시뮬레이션에서는 로봇이 이동하거나 위성군이 시간에 따라 변화하면서 발생하는 위성 기하구조의 변화를 재현할 수 있다.

전리층(Ionosphere)은 GNSS 오차를 발생시키는 주요 대기 요인 중 하나이다. 위성의 무선 신호는 상층 대기의 이온화 영역을 통과하며, 이 과정에서 전파 속도는 전자 밀도(Electron Density)와 신호 주파수에 영향을 받는다. 이에 따라 의사거리 측정값에 영향을 주는 추가적인 지연이 발생한다. 전리층 오차(Ionospheric Error)는 위치, 시간, 태양 활동(Solar Activity), 위성 고도각(Satellite Elevation Angle), 주파수에 따라 변화하므로 현실적인 GNSS 시뮬레이션에서 중요한 요소이다.

이중 주파수(Dual-Frequency) 및 다중 주파수(Multi-Frequency) 수신기는 전리층 지연이 주파수에 따라 달라지는 특성을 이용하여 전리층 오차를 줄일 수 있다. 단순한 시뮬레이터에서는 잔여 전리층 영향을 고도각 의존적 오차(Elevation-Dependent Error) 또는 확률적 거리 오차(Stochastic Range Error)로 표현할 수 있으며, 고충실도 모델에서는 대기 파라미터를 이용하여 각 위성의 지연을 계산할 수 있다. 모델의 복잡도는 일반적인 로봇 내비게이션 시험인지 상세한 GNSS 알고리즘 개발인지에 따라 결정해야 한다.

대류권(Troposphere) 역시 위성 신호의 전파에 영향을 준다. 전리층 효과와 달리 대류권 지연(Tropospheric Delay)은 주로 대기압, 온도, 습도, 하층 대기를 통과하는 신호 경로의 길이와 관련된다. 수평선 부근의 저고도 위성은 더 긴 대기 경로를 통과하기 때문에 일반적으로 더 큰 지연이 발생한다. 따라서 대류권 모델(Tropospheric Model)은 위성 고도각과 환경 조건을 함께 사용하여 물리적으로 보다 의미 있는 의사거리 오차를 생성할 수 있다.

위성 시계 및 궤도 오차(Satellite Clock and Orbit Error)는 추가적인 불확실성을 발생시킨다. 항법 메시지(Navigation Message)에는 위성 위치와 시계 상태의 추정값이 포함되지만 이러한 값이 완벽하게 정확하지는 않다. 위성 위치 또는 시간의 작은 오차도 수신기의 거리 측정값에 전달된다. 대부분의 로보틱스 시뮬레이션에서는 이러한 영향을 통계적으로 표현할 수 있지만 고충실도 GNSS 시뮬레이션에서는 개별 위성의 천체력 오차(Ephemeris Error)와 시계 오차를 시간에 따라 변화하는 과정으로 모델링할 수 있다.

수신기 시계 편향(Receiver Clock Bias)은 매우 작은 시간 오차도 빛의 속도로 환산하면 상당한 거리 오차에 해당하기 때문에 특히 중요하다. GNSS 위치 결정 알고리즘은 수신기 위치와 함께 이러한 시계 오프셋(Clock Offset)을 추정한다. 따라서 의사거리 수준 시뮬레이터(Pseudorange-Level Simulator)는 수신기 시계 편향과 필요에 따라 시계 드리프트(Clock Drift)를 포함해야 한다. 이를 통해 위치 결정 및 필터링 알고리즘이 실제 GNSS 수신기가 제공하는 것과 보다 유사한 측정 구조에서 동작하도록 할 수 있다.

수신기 측정 노이즈(Receiver Measurement Noise)는 전자회로, 신호 추적(Signal Tracking), 열적 영향, 안테나 특성, 신호 강도에서 발생한다. 이는 의사거리, 반송파 위상(Carrier Phase), 도플러(Doppler), 또는 최종 위치 측정값에 추가되는 랜덤 노이즈(Random Noise)로 표현할 수 있다. 노이즈 크기는 수신기 품질과 운용 조건을 반영해야 한다. 비현실적으로 작은 독립 가우시안 노이즈(Independent Gaussian Noise)를 사용하면 위치 추정 및 센서 융합 알고리즘의 성능을 실제보다 크게 평가할 수 있다.

다중경로(Multipath)는 위성 신호가 직접 경로와 반사 경로를 통해 동시에 수신기에 도달할 때 발생한다. 건물, 차량, 벽, 금속 구조물, 지형 및 기타 표면은 GNSS 신호를 반사하여 의사거리 또는 반송파 위상 측정값을 왜곡할 수 있다. 다중경로는 반사 신호가 강하고 지속적으로 발생할 수 있는 도심 협곡(Urban Canyon), 산업 현장, 항만, 대형 구조물 주변에서 특히 중요한 영향을 미친다.

비가시선 수신(Non-Line-of-Sight Reception)은 위성의 직접 신호가 차단되었지만 반사 신호는 여전히 감지될 때 발생한다. 이 경우 단순한 영평균 노이즈(Zero-Mean Noise)가 아니라 크고 편향된 오차가 발생할 수 있다. 기하구조 인식 시뮬레이터(Geometry-Aware Simulator)는 건물, 지형, 식생 또는 로봇 구조물을 대상으로 레이 캐스팅(Ray Casting)을 수행하여 위성 가시성을 판단할 수 있다. 차단된 위성은 모델링하는 수신기와 전파 조건에 따라 완전히 제거하거나 성능이 저하된 측정값을 생성하도록 설정할 수 있다.

위성 고도각(Satellite Elevation)은 가시성과 측정 품질 모두에 영향을 준다. 저고도 위성은 더 긴 대기 경로를 통과하며 장애물과 다중경로의 영향을 받을 가능성도 높다. 실제 수신기는 일반적으로 고도각 마스크(Elevation Mask)를 적용하여 설정된 각도보다 낮은 위성을 제외한다. 시뮬레이션에서도 각각의 신호가 항법 해에 사용되는지를 결정하기 전에 수신기를 기준으로 위성의 고도각과 방위각(Azimuth)을 계산하여 이러한 동작을 재현할 수 있다.

가시 위성 수(Number of Visible Satellites)는 환경과 시간에 따라 변화한다. 개방된 실외 공간에서는 양호한 위성 커버리지(Satellite Coverage)를 확보할 수 있지만 높은 건물 사이의 도로, 터널, 실내 공간, 숲, 구조물 아래에서는 위성 가용성이 크게 감소할 수 있다. 따라서 GNSS 시뮬레이션은 단순한 위치 오차뿐만 아니라 기하구조 악화(Degraded Geometry), 간헐적인 측정(Intermittent Measurement), 위성 기반 위치 결정이 불가능한 완전한 신호 단절(Complete Outage)도 모델링해야 한다.

위치 오차(Position Error)는 각 샘플마다 독립적으로 발생하기보다 시간적으로 상관된 경우가 많다. 대기 상태, 다중경로, 수신기 편향, 위성 기하구조는 수초에서 수분 동안 지속될 수 있으며 이에 따라 추정 위치가 천천히 드리프트하거나 특정 방향으로 지속적인 오프셋을 가질 수 있다. 시간 상관 확률 모델(Time-Correlated Stochastic Model)은 매 갱신마다 완전히 독립적인 무작위 위치 오프셋을 생성하는 방식보다 이러한 동작을 현실적으로 재현할 수 있다.

GNSS 수신기는 위치뿐만 아니라 다양한 정보를 제공할 수 있다. 장치에 따라 출력에는 고도, 지상 속도(Ground Velocity), 진행 방향(Course), 위성 상태(Satellite Status), 정밀도 저하율, 의사거리, 반송파 위상, 도플러, 측정 공분산(Measurement Covariance)이 포함될 수 있다. 시뮬레이터는 내비게이션 스택(Navigation Stack)이 요구하는 측정 수준을 제공해야 한다. 단순한 모바일 로봇은 위치와 속도만 필요할 수 있지만 고급 상태 추정기(Advanced Estimator)는 원시 위성 관측값(Raw Satellite Observation)을 요구할 수 있다.

도플러 측정(Doppler Measurement)은 위성과 수신기 사이의 상대 운동으로 발생하는 반송파 주파수(Carrier Frequency)의 변화를 관측하여 수신기의 속도 정보를 제공할 수 있다. 시뮬레이션 도플러는 위성과 로봇의 속도에 시계 효과와 측정 노이즈를 결합하여 생성할 수 있다. 도플러에서 얻은 속도는 위치 차분으로 계산한 속도와 다른 오차 특성을 가질 수 있으므로 GNSS-IMU 융합(GNSS-IMU Fusion)과 동적 내비게이션(Dynamic Navigation)에 유용하다.

고정밀 시스템(High-Precision System)은 차분 GNSS(Differential GNSS), 실시간 이동측위(Real-Time Kinematic, RTK) 또는 관련 보정 방식으로 표현할 수 있다. 이러한 기술은 추가적인 관측값이나 보정 정보를 이용하여 공통 위성 오차, 대기 오차, 시계 오차를 줄인다. 필요에 따라 시뮬레이션에서는 향상된 정확도, 모호정수 결정(Ambiguity Resolution), 보정 지연(Correction Latency), 고정해(Fixed), 유동해(Float), 성능 저하 해(Degraded Solution) 사이의 전환을 표현할 수 있다. 이는 센티미터 수준의 실외 위치 정밀도를 요구하는 로봇에 유용하다.

GNSS 측정값은 위성 기반 위치 정보만으로는 노이즈가 크거나 간헐적이며 완전히 사용할 수 없는 상황이 발생할 수 있기 때문에 일반적으로 IMU 및 휠 오도메트리(Wheel Odometry)와 융합한다. IMU는 고주파 상대 운동 정보를 제공하고 GNSS는 누적되는 관성 드리프트(Inertial Drift)를 제한하는 전역 기준 보정값을 제공한다. 시뮬레이션에서는 현실적인 갱신 주기, 타임스탬프(Timestamp), 지연시간, 신호 단절, 불확실성을 유지하여 칼만 필터(Kalman Filter), 팩터 그래프(Factor Graph), 기타 상태 추정기가 실제와 유사한 융합 조건을 경험하도록 해야 한다.

안테나 배치(Antenna Placement) 역시 측정값에 영향을 준다. GNSS 안테나는 로봇의 기준점에서 떨어진 위치에 장착될 수 있으며 이 경우 회전 운동 중 레버 암(Lever Arm)의 영향이 중요해진다. 또한 주변의 로봇 구조물이 위성 신호를 차단하거나 반사할 수 있다. 따라서 고충실도 시뮬레이터는 측정값이 항상 로봇의 기하학적 중심에서 생성된다고 가정하는 대신 안테나 자세(Antenna Pose), 차량 방향, 주변 기하구조, 위성 가시선(Line of Sight)을 모델링할 수 있다.

정답 정보(Ground-Truth Information)를 사용할 수 있다는 점은 위치 추정 성능 평가에서 시뮬레이션의 중요한 장점이다. 정확한 로봇 궤적(Robot Trajectory)을 이상적인 거리, 가시 위성, 대기 지연(Atmospheric Delay), 다중경로 조건, 수신기 오차, 최종 노이즈 측정값과 함께 기록할 수 있다. 이를 통해 개발자는 위성 가용성, 환경 전파, 센서 모델링, 좌표 변환, 시간 처리 또는 상태 추정 알고리즘에서 발생한 실패 원인을 구분하여 분석할 수 있다.

도메인 랜덤화(Domain Randomization)는 시뮬레이션 실행마다 대기 지연, 위성 가시성, 수신기 노이즈, 시계 드리프트, 다중경로 강도(Multipath Severity), 차폐 패턴(Obstruction Pattern), 갱신 주파수, 지연시간, 안테나 자세, 신호 단절 지속시간(Outage Duration)을 변화시킬 수 있다. 이를 통해 내비게이션 시스템을 더욱 다양한 운용 조건에 노출하고 비현실적으로 안정적인 위성 측정값에 의존하는 것을 방지할 수 있다. 랜덤화는 지리적으로 다양한 환경에 자율 시스템을 배치하기 위한 준비 과정에서 특히 유용하다.

검증(Validation)은 동일하거나 유사한 환경에서 시뮬레이션 GNSS와 실제 GNSS의 동작을 비교하는 방식으로 수행해야 한다. 주요 평가 특성에는 수평 및 수직 위치 오차(Horizontal and Vertical Position Error), 속도 오차, 위성 수, 정밀도 저하율, 신호 단절 빈도(Outage Frequency), 시간적 상관관계, 신호 손실 이후의 복구 특성이 포함된다. 공칭 정확도만 일치하는 모델은 도심 다중경로나 위성 가시성 저하를 제대로 재현하지 못할 수 있으므로 개방 환경(Open-Sky Environment)과 차폐 환경(Obstructed Environment)을 모두 평가해야 한다.

실용적인 GNSS 시뮬레이션 아키텍처(GNSS Simulation Architecture)는 위성군 기하구조(Satellite Constellation Geometry), 수신기 및 안테나 상태, 좌표 변환, 기하학적 거리, 대기 전파, 시계 오차, 다중경로, 차폐(Obstruction), 측정 노이즈, 시간 특성, 항법 해 생성을 연결한다. 필요한 충실도(Fidelity)는 기본 위치 추정, GNSS-IMU 융합, 자율 내비게이션, RTK 개발, 상세 수신기 평가 중 어떤 목적을 가지는지에 따라 결정된다. 적절한 모델링은 강건한 내비게이션(Robust Navigation)과 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 현실적인 전역 위치 데이터를 제공한다.

## 04.09. Sensor Synchronization and Timestamp in Simulation [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 동기화(Sensor Synchronization)는 서로 다른 시뮬레이션 센서에서 생성된 측정값이 올바른 물리적 시점(Physical Moment)에 대응하도록 보장한다. 로봇 시스템은 카메라, 라이다(LiDAR), 레이더(Radar), 관성 측정 장치(IMU), 위성항법시스템(GNSS), 휠 오도메트리(Wheel Odometry), 관절 엔코더(Joint Encoder), 힘 센서(Force Sensor)를 서로 다른 갱신 주기로 결합할 수 있다. 일관된 타임스탬프(Timestamp)와 시간 동작(Timing Behavior)이 없으면 개별 센서 모델이 정확하더라도 인지, 위치 추정, 매핑, 제어 결과에 오류가 발생할 수 있다.

시뮬레이션은 일반적으로 물리 엔진(Physics Engine) 또는 시뮬레이션 엔진이 관리하는 전역 시뮬레이션 시계(Global Simulation Clock)에 따라 진행된다. 이 시계는 강체 동역학(Rigid-Body Dynamics), 센서 데이터 생성, 제어기 실행, 데이터 기록을 위한 공통 시간 기준(Common Temporal Reference)을 제공한다. 계산 시간과 시뮬레이션 내부의 물리적 시간이 반드시 동일한 속도로 진행되는 것은 아니므로 센서 타임스탬프는 호스트 컴퓨터 시계(Host Computer Clock)를 독립적으로 사용하는 대신 이 공통 기준과 연결되어야 한다.

시뮬레이션 시간(Simulation Time)은 실제 경과 시간(Wall-Clock Time)과 근본적으로 다르다. 계산량이 많은 장면에서는 로봇 동작 1초를 시뮬레이션하는 데 실제 처리 시간이 수초 이상 필요할 수 있으며, 단순한 시뮬레이션은 실시간보다 빠르게 실행될 수도 있다. 시뮬레이터의 일시 정지, 단계 실행(Step Execution), 가속 실행은 두 시간 체계를 더욱 분리할 수 있다. 따라서 가상 세계 내부의 시간적 관계를 표현하려는 알고리즘은 시뮬레이션 타임스탬프를 사용해야 한다.

서로 다른 센서는 본질적으로 서로 다른 샘플링 주파수(Sampling Frequency)로 동작한다. IMU는 초당 수백 또는 수천 개의 측정값을 생성할 수 있지만 카메라는 일반적으로 초당 수십 프레임(Frame), GNSS는 초당 몇 회 정도만 갱신될 수 있다. 동기화 시스템은 모든 센서가 매 물리 시뮬레이션 단계마다 데이터를 생성하도록 강제하는 대신 이러한 독립적인 갱신 주기(Independent Update Rate)를 유지해야 한다. 이를 통해 실제 로봇에서 발생하는 비동기 측정 스트림(Asynchronous Measurement Stream)을 재현할 수 있다.

물리 시간 간격(Physics Timestep)은 기반 시뮬레이션 상태가 갱신되는 시점을 정의하지만 센서 샘플링 주기(Sensor Sampling Period)는 관측값이 생성되는 시점을 결정한다. 이 두 주기가 서로 정확한 정수배 관계가 아닐 수도 있다. 따라서 시뮬레이터는 센서 이벤트(Sensor Event)를 신중하게 스케줄링해야 하며 필요한 경우 물리 상태 사이의 로봇 상태를 보간(Interpolation)해야 한다. 그렇지 않으면 시간 양자화(Timing Quantization)로 인해 고주파 센서 스트림에 인위적인 지터(Jitter)나 체계적인 타임스탬프 오차가 발생할 수 있다.

타임스탬프(Timestamp)는 단순히 데이터가 소프트웨어에 제공된 시간이 아니라 해당 측정값이 나타내는 실제 물리적 시점(Physical Time)을 식별해야 한다. 센싱, 처리, 통신, 버퍼링(Buffering) 과정에서 지연시간(Latency)이 발생하기 때문에 이 두 시점은 서로 다를 수 있다. 측정 시각(Measurement Time)과 발행 또는 도착 시각(Publication or Arrival Time)을 분리하면 데이터가 이전의 물리 상태를 나타내지만 상태 추정기에는 나중에 전달되는 현실적인 센서 파이프라인을 재현할 수 있다.

센서 지연시간(Sensor Latency)은 여러 요소로 구성될 수 있다. 노출(Exposure), 스캐닝(Scanning), 신호 적분(Signal Integration), 내부 처리(Internal Processing), 데이터 전송(Data Transfer), 미들웨어 큐(Middleware Queue), 소프트웨어 스케줄링(Software Scheduling)이 각각 지연에 기여할 수 있다. 카메라 이미지는 노출과 영상 처리 시간이 필요하며 라이다는 시야 영역을 스캔하는 데 시간이 필요하다. 시뮬레이션에서는 이러한 지연을 개별적으로 모델링하여 후속 알고리즘이 실제 센싱 시스템과 유사한 시간적 동작을 경험하도록 할 수 있다.

지터(Jitter)는 측정마다 지연시간 또는 샘플링 시점이 변화하는 현상을 의미한다. 센서가 공칭 갱신 주파수(Nominal Update Frequency)를 가지더라도 운영체제 스케줄링, 통신 트래픽, 장치 전자회로, 처리 부하에 따라 정확한 데이터 도착 시간이 달라질 수 있다. 시뮬레이션 지터는 제한된 범위 또는 확률적 시간 변화(Stochastic Timing Variation)로 모델링할 수 있다. 이를 통해 센서 데이터가 완벽하게 주기적으로 전달되지 않는 상황에서도 상태 추정기와 제어기가 안정적으로 동작하는지 시험할 수 있다.

시계 오프셋(Clock Offset)은 두 장치가 일정하거나 천천히 변화하는 차이를 가진 시간 기준을 사용할 때 발생한다. 시계 드리프트(Clock Drift)는 두 시계의 진행 속도가 미세하게 달라 시간이 지나면서 오프셋이 증가하는 현상이다. 실제 분산 로봇 시스템(Distributed Robotic System)은 센서, 임베디드 컴퓨터, 엣지 컴퓨터(Edge Computer), 네트워크 장치에 서로 다른 시계를 포함할 수 있다. 시뮬레이션에서는 오프셋과 드리프트를 재현하여 타임스탬프 보정 및 동기화 알고리즘을 평가할 수 있다.

실제 하드웨어 시스템에서는 일반적으로 네트워크 시간 동기화(Network Time Synchronization), 정밀 시간 프로토콜(Precision Timing Protocol), GNSS 기반 시간(GNSS-Derived Time), 하드웨어 트리거 신호(Hardware Trigger Signal)와 같은 방법을 이용하여 시계를 동기화한다. 시뮬레이션 내부에서 모든 동기화 프로토콜을 직접 재현할 필요는 없지만 시간 충실도(Timing Fidelity)가 중요한 경우 이러한 방법으로 얻을 수 있는 동기화 정확도와 잔여 오차(Residual Error)를 표현해야 한다. 이를 통해 동기화된 실제 하드웨어를 대상으로 설계된 소프트웨어를 현실적인 시간 불확실성 조건에서 시험할 수 있다.

카메라 시간 처리(Camera Timing)는 이미지가 순간적인 측정값이 아니라 일정한 노출 구간(Exposure Interval)에 걸쳐 빛을 적분한 결과이기 때문에 특별한 주의가 필요하다. 글로벌 셔터 카메라(Global-Shutter Camera)는 모든 픽셀을 거의 동일한 시간 구간에 노출하지만 롤링 셔터 카메라(Rolling-Shutter Camera)는 서로 다른 이미지 행을 서로 다른 시점에 노출한다. 로봇이나 객체가 빠르게 움직이면 이러한 시간 차이가 모션 블러(Motion Blur) 또는 기하학적 왜곡(Geometric Distortion)을 발생시키므로 시각 내비게이션을 평가할 때 이를 모델링해야 한다.

라이다 측정값(LiDAR Measurement) 역시 시간에 걸쳐 분산되어 생성된다. 회전형 또는 스캐닝 라이다는 포인트 클라우드(Point Cloud)의 모든 점을 동시에 획득하지 않으며 각각의 빔(Beam)은 하나의 스캔 과정에서 서로 다른 시점에 방출된다. 스캐닝 도중 로봇이 이동하면 생성된 포인트 클라우드에 모션 왜곡(Motion Distortion)이 발생한다. 라이다 디스큐잉(LiDAR Deskewing), 오도메트리, 매핑, 센서 융합을 수행하는 알고리즘을 평가하려면 포인트별 또는 열(Column)별 시간 정보를 정확하게 유지해야 한다.

레이더(Radar)는 처프(Chirp), 프레임(Frame), 적분 구간(Integration Interval), 신호 처리에 의해 결정되는 고유한 시간 구조(Temporal Structure)를 가진다. 거리 및 도플러 측정(Range and Doppler Measurement)은 하나의 순간적인 상태가 아니라 일정한 시간 구간 동안 누적된 관측값을 나타낼 수 있다. 반면 IMU는 일반적으로 훨씬 높은 주파수로 측정값을 제공한다. 따라서 레이더를 IMU 또는 카메라 데이터와 동기화하려면 측정 시각, 프레임 경계(Frame Boundary), 처리 지연을 명확하게 정의해야 한다.

GNSS는 일반적으로 낮은 갱신 주파수로 동작하며 수신기 처리 지연(Receiver Processing Delay)이 발생할 수 있다. 따라서 내비게이션 필터(Navigation Filter)에 도착한 위치와 속도 측정값은 가장 최근의 IMU 샘플보다 이전 상태를 나타낼 수 있다. 상태 추정기가 이러한 지연을 무시하면 잘못된 상태에 보정값을 적용할 수 있다. 긴밀하게 결합된 내비게이션 시스템(Tightly Integrated Navigation System)을 평가하려면 시뮬레이션 GNSS 시간 모델에 현실적인 타임스탬프와 지연시간을 포함해야 한다.

서로 다른 센서의 측정값이 정확하게 동일한 타임스탬프를 가지는 경우는 드물기 때문에 근사 동기화(Approximate Synchronization)가 필요한 경우가 많다. 융합 시스템은 측정값의 타임스탬프가 허용 가능한 시간 윈도우(Temporal Window) 내부에 존재할 때 서로 연관시킬 수 있다. 적절한 허용 오차(Tolerance)는 센서 주파수, 플랫폼 동역학(Platform Dynamics), 알고리즘 요구사항에 따라 달라진다. 천천히 이동하는 로봇은 고속 차량, 무인항공기 또는 빠르게 움직이는 매니퓰레이터보다 더 큰 시간 오차를 허용할 수 있다.

보간(Interpolation)은 센서의 정확한 측정 시점이 사용 가능한 시뮬레이션 상태 사이에 위치할 때 해당 시점의 로봇 상태를 추정하는 데 사용할 수 있다. 짧은 시간 간격의 위치에는 선형 보간(Linear Interpolation)이 충분할 수 있지만 자세(Orientation)는 일반적으로 적절한 회전 보간(Rotational Interpolation) 방법을 사용해야 한다. 정확한 시간 보간은 이산화 인공물(Discretization Artifact)을 감소시키고 독립적인 갱신 스케줄을 가진 센서들이 연속적인 로봇 운동을 일관되게 관측하도록 한다.

외삽(Extrapolation)은 알고리즘이 현재 상태를 필요로 하지만 가장 최신의 센서 측정값이 이전 시점을 나타내는 경우 필요할 수 있다. 운동 모델(Motion Model)이나 IMU 적분(IMU Integration)을 이용하여 상태 추정값을 미래 방향으로 전파할 수 있지만 외삽 시간이 길어질수록 불확실성이 증가한다. 현실적인 지연시간을 포함하는 시뮬레이션은 이러한 예측 메커니즘을 평가하고 지연시간이 위치 추정과 제어 성능에 미치는 영향을 분석하기 위한 통제된 환경을 제공한다.

타임스탬프 오차(Timestamp Error)는 공간 정렬(Spatial Alignment)에 직접적인 영향을 준다. 이동 중인 로봇이 서로 다른 시점에 카메라와 라이다로 객체를 관측했음에도 두 측정값을 동시에 획득한 것으로 처리하면 이미지에 투영된 포인트 클라우드가 정확하게 정렬되지 않을 수 있다. 유사한 문제는 레이더-카메라 융합(Radar-Camera Fusion), GNSS-IMU 추정, 매핑, 로봇 조작에서도 발생한다. 일반적으로 로봇의 움직임이 빠를수록 동일한 시간 오차가 더 큰 공간적 불일치(Spatial Inconsistency)로 변환된다.

센서 외부 보정(Sensor Extrinsic Calibration)과 시간 보정(Temporal Calibration)은 서로 밀접하게 연관되어 있다. 외부 보정은 센서 사이의 공간 변환(Spatial Transformation)을 추정하고 시간 보정은 상대적인 시간 오프셋(Relative Timing Offset)을 추정한다. 플랫폼이 움직이는 경우 시간 오차가 공간적 보정 오차와 유사하게 나타날 수 있다. 시뮬레이션에서는 공간 및 시간 파라미터를 독립적으로 제어할 수 있으므로 보정 알고리즘을 시험하고 관측된 불일치가 기하구조 또는 시간 문제에서 발생하는지를 분석하는 데 유용하다.

메시지 순서(Message Ordering)는 여러 비동기 데이터 스트림이 미들웨어 또는 네트워크를 통해 전송될 때 중요하다. 측정값은 실제 획득된 순서와 다른 순서로 도착할 수 있다. 강건한 시스템(Robust System)은 시간적 순서를 구성할 때 도착 순서가 아니라 타임스탬프를 사용해야 한다. 시뮬레이션에서는 가변 지연(Variable Delay)과 순서가 뒤바뀐 전달(Out-of-Order Delivery)을 의도적으로 발생시켜 버퍼링, 재정렬(Reordering), 지연 측정 처리(Delayed-Measurement Handling) 기능을 시험할 수 있다.

데이터 버퍼(Data Buffer)는 서로 호환되는 타임스탬프를 가진 관측값을 연결할 수 있도록 최근 센서 측정값을 유지한다. 버퍼 지속시간(Buffer Duration)은 예상되는 지연시간과 데이터 순서 변경을 수용할 만큼 충분히 길어야 하지만 메모리 사용량과 처리 지연이 과도하게 증가할 정도로 길어서는 안 된다. 시뮬레이션 센서 파이프라인은 실제 버퍼링 요구사항을 재현하고 동기화 정책(Synchronization Policy)이 지나치게 많은 데이터를 폐기하거나 부적절한 시점의 측정값을 결합하는지를 확인할 수 있다.

결정론적 시뮬레이션(Deterministic Simulation)은 동기화 문제를 디버깅할 때 유용하다. 고정된 물리 시간 간격, 제어된 센서 스케줄, 반복 가능한 랜덤 시드(Random Seed), 기록된 이벤트 시간을 사용하면 시간 관련 오류를 동일한 조건에서 정확하게 재현할 수 있다. 개발자는 정답 획득 시각(Ground-Truth Acquisition Time), 발행 시각(Publication Time), 상태 추정기 처리 시각(Estimator Processing Time)을 비교함으로써 실제 하드웨어에서는 일관되게 재현하기 어려운 동기화 오류를 보다 쉽게 분리하여 분석할 수 있다.

합성 데이터셋(Synthetic Dataset)은 센서 값뿐만 아니라 시간 메타데이터(Timing Metadata)도 유지해야 한다. 각각의 측정값에는 획득 타임스탬프(Acquisition Timestamp), 시퀀스 식별자(Sequence Identifier), 프레임 식별자(Frame Identifier), 노출 또는 스캔 시간 정보, 필요에 따라 발행 시각을 포함할 수 있다. 정답 상태(Ground-Truth State) 역시 동일한 시간 기준을 이용하여 타임스탬프를 기록해야 한다. 이를 통해 데이터셋을 현실적인 재생(Replay), 센서 융합 연구, 시간 보정, 모션 보상(Motion Compensation), 벤치마킹(Benchmarking)에 활용할 수 있다.

도메인 랜덤화(Domain Randomization)는 시각적 및 물리적 특성뿐만 아니라 시간 파라미터(Timing Parameter)에도 적용할 수 있다. 센서 주파수, 지연시간, 지터, 시계 오프셋, 시계 드리프트, 패킷 지연(Packet Delay), 측정값 손실(Dropped Measurement), 동기화 정확도를 시뮬레이션 에피소드마다 변화시킬 수 있다. 시간 랜덤화(Timing Randomization)는 알고리즘이 완벽하게 동기화된 센서를 가정하는 것을 방지하며 소프트웨어를 실제 분산 컴퓨팅 시스템(Distributed Computing System)으로 전이할 때 강건성(Robustness)을 향상시킬 수 있다.

검증(Validation)은 시뮬레이션의 시간 동작과 실제 센서 로그(Physical Sensor Log)를 비교하는 방식으로 수행해야 한다. 중요한 특성에는 갱신 주파수, 타임스탬프 간격(Timestamp Interval), 지연시간 분포(Latency Distribution), 지터, 시계 오프셋, 드리프트, 메시지 순서, 프레임 손실(Dropped Frame), 동기화 정확도가 포함된다. 특히 동적 시험(Dynamic Test)은 시간 오차가 공간 정렬 불일치, 모션 왜곡 또는 위치 추정 및 센서 융합 성능 저하로 나타나기 때문에 유용하다.

실용적인 동기화 아키텍처(Synchronization Architecture)는 시뮬레이션 시계, 물리 시간 간격, 독립 센서 스케줄러(Independent Sensor Scheduler), 획득 타임스탬프, 센서별 지연시간, 시계 모델(Clock Model), 통신 지연, 버퍼, 후속 센서 융합 알고리즘을 연결한다. 시간 정보는 측정값 생성 이후 단순히 추가되는 메타데이터가 아니라 센서 모델 자체의 일부로 다루어야 한다. 정확한 동기화는 신뢰할 수 있는 인지, 위치 추정, 매핑, 제어, 데이터셋 생성 및 시뮬레이션-현실 전이(Sim2Real Transfer)를 가능하게 한다.

## 04.10. Photorealistic Sensor Rendering Ray Tracing RTX

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

포토리얼리스틱 센서 렌더링(Photorealistic Sensor Rendering)은 실제 로봇 센서가 물리적 환경에서 관측하는 시각적 외관과 광학적 동작을 재현하는 것을 목표로 한다. 장면을 단순히 기하구조, 단순화된 셰이딩(Shading), 이상적인 깊이 값으로만 표현하는 대신, 고충실도 렌더링(High-Fidelity Rendering)은 빛의 전달(Light Transport), 재질 반응(Material Response), 반사(Reflection), 그림자(Shadow), 투명도(Transparency), 카메라 특성을 모델링한다. 이를 통해 시뮬레이션은 인지 개발(Perception Development), 합성 데이터셋(Synthetic Dataset), 시뮬레이션-현실 전이(Sim2Real) 평가에 적합한 센서 데이터를 생성할 수 있다.

전통적인 래스터화(Rasterization)는 기하학적 프리미티브(Geometric Primitive)를 이미지 평면에 투영하고 가시적인 표면을 효율적으로 결정하므로 실시간 시뮬레이션에 적합하다. 그러나 많은 광학 현상은 래스터화만으로 정확하게 재현하기 어렵다. 레이 트레이싱(Ray Tracing)은 가상 장면을 통해 광선을 추적하고 광선과 표면의 상호작용을 평가하므로 반사, 굴절, 그림자, 간접 조명(Indirect Illumination), 복잡한 재질 효과를 보다 물리적으로 의미 있는 방식으로 표현할 수 있다.

레이 트레이싱은 가상 카메라에서 이미지의 각 픽셀을 통과하여 시뮬레이션 환경으로 향하는 광선을 생성하는 것에서 시작한다. 각 광선은 장면 기하구조와의 교차 여부를 검사하여 가장 가까운 교차점을 결정한다. 이후 렌더러는 재질 특성, 조명, 표면 방향, 그리고 필요한 경우 추가적인 반사 또는 투과 광선을 평가한다. 이 과정은 장면 기하구조를 광학적 관측값과 직접 연결하며 현실적인 RGB 및 능동 광학 센서(Active Optical Sensor) 시뮬레이션을 위한 기반을 제공한다.

패스 트레이싱(Path Tracing)은 표면, 광원, 카메라 사이의 여러 광 전달 경로(Light Transport Path)를 통계적으로 샘플링하여 레이 트레이싱을 확장한다. 광선은 하나의 픽셀에 기여하기 전에 여러 번의 확산(Diffuse) 또는 정반사(Specular) 상호작용을 수행할 수 있다. 충분한 샘플을 사용하면 패스 트레이싱은 표면 사이의 간접 조명과 색상 전달(Color Transfer)을 포함한 전역 조명(Global Illumination)을 근사할 수 있다. 이러한 물리적 충실도는 합성 인지 데이터(Synthetic Perception Data)에 유용하지만 기존 렌더링보다 계산 비용이 상당히 높을 수 있다.

RTX급 하드웨어(RTX-Class Hardware)는 전용 레이 트레이싱 처리 자원과 고도로 병렬화된 GPU 계산을 이용하여 레이 트레이싱을 가속한다. 하드웨어 가속을 통해 대량의 광선-장면 교차 검사(Ray-Scene Intersection Test)를 수행하면서 상호작용 또는 준실시간 시뮬레이션 속도를 유지할 수 있다. 로보틱스 시뮬레이션에서는 RTX 렌더링을 통해 물리 기반 재질, 복잡한 기하구조, 동적 객체, 고급 조명을 결합하면서도 반복적인 개발과 대규모 합성 데이터 생성을 지원할 수 있다.

물리 기반 렌더링(Physically Based Rendering)은 실제 표면이 빛과 상호작용하는 방식을 근사하도록 설계된 재질 모델(Material Model)을 사용한다. 기본 색상(Base Color), 거칠기(Roughness), 금속성(Metallic Behavior), 반사율(Reflectance), 투명도(Transparency), 표면 법선(Surface Normal)과 같은 파라미터가 최종 외관에 영향을 준다. 광택이 있는 금속, 도장된 벽, 고무 타이어, 유리 패널, 거친 콘크리트 표면은 동일한 조명에서도 서로 다르게 반응해야 한다. 따라서 정확한 재질 모델링은 포토리얼리스틱 센서 관측값을 생성하는 데 필수적이다.

조명(Lighting)은 렌더링된 센서 데이터의 현실성을 크게 결정한다. 시뮬레이션 환경에는 햇빛, 하늘 조명(Sky Illumination), 인공 조명, 발광 표면(Emissive Surface), 국소 광원(Local Light Source)이 포함될 수 있다. 조명의 세기, 방향, 스펙트럼 특성, 공간적 분포는 그림자, 대비, 반사, 노출(Exposure)에 영향을 준다. 동적 조명 모델을 사용하면 인지 시스템을 주간, 야간, 실내, 실외, 역광(Backlight), 저조도(Low Illumination) 조건에서 평가할 수 있다.

전역 조명(Global Illumination)은 빛이 환경과 간접적으로 상호작용한 이후 센서에 도달하는 과정을 표현한다. 바닥, 벽, 객체, 주변 구조물에서 반사된 빛은 직접적인 조명을 거의 받지 못하는 영역까지 밝힐 수 있다. 이러한 상호작용을 무시하면 비현실적인 대비와 그림자 경계가 생성될 수 있다. 레이 트레이싱 또는 패스 트레이싱 기반 전역 조명은 이러한 2차 효과(Secondary Effect)를 재현하여 실제 물리 환경과 더욱 가까운 시각 조건을 생성할 수 있다.

반사 모델링(Reflection Modeling)은 유리, 광택 바닥, 금속 기계, 젖은 표면, 차량, 창문 등이 포함된 환경에서 중요하다. 반사된 객체는 모호한 시각적 특징(Ambiguous Visual Feature)을 생성할 수 있으며 객체 분할(Segmentation), 검출(Detection), 깊이 추정(Depth Estimation), 위치 추정(Localization) 알고리즘에 영향을 줄 수 있다. 레이 트레이싱은 기하학적으로 일관된 반사를 자연스럽게 지원하므로 단순화된 렌더러에서 생략될 수 있는 어려운 조건에 인지 시스템을 노출할 수 있다.

굴절(Refraction)과 투과(Transmission)는 광선이 투명 또는 반투명 재질을 통과할 때 중요해진다. 유리 패널, 렌즈, 보호 커버, 물, 투명 플라스틱은 광선의 방향과 세기를 변화시킬 수 있다. 목표 센서에 따라 시뮬레이터는 굴절률(Refractive Index), 흡수(Absorption), 투과율(Transmission), 표면 경계(Surface Interface)를 이용하여 이러한 효과를 모델링할 수 있다. 이러한 동작은 로봇이 창문, 디스플레이 커버, 투명 객체 주변에서 작동할 때 특히 중요하다.

그림자(Shadow)는 유용한 기하학적 단서를 제공하지만 동시에 영상 밝기에 어려운 변화를 발생시킨다. 작거나 먼 광원은 선명한 그림자를 만들 수 있으며, 확장된 광원은 부드러운 반음영(Soft Penumbra)을 생성한다. 이동하는 로봇, 사람, 차량, 기계는 객체의 정체성이 변하지 않더라도 외관을 변화시키는 동적 그림자를 만들 수 있다. 물리적으로 일관된 그림자 렌더링은 인지 알고리즘이 조명 변화와 실제 장면 구조를 구분하는 데 도움을 준다.

텍스처 품질(Texture Quality)과 기하학적 세부 수준(Geometric Detail)은 함께 포토리얼리즘에 영향을 준다. 고해상도 텍스처는 긁힘, 얼룩, 라벨, 도로 표시, 표면 마모와 같은 외관 특징을 표현할 수 있으며, 상세한 기하구조는 모서리와 3차원 구조를 표현한다. 노멀 맵(Normal Map), 디스플레이스먼트 맵(Displacement Map), 절차적 재질(Procedural Material)은 매우 고밀도의 메시(Mesh)를 사용하지 않고도 추가적인 표면 세부 정보를 제공할 수 있다. 선택하는 표현 방식은 시각적 충실도, 메모리 사용량, 렌더링 성능 사이의 균형을 고려해야 한다.

카메라 시뮬레이션(Camera Simulation)은 장면 렌더링을 넘어 센서의 광학 및 전자적 특성까지 포함해야 한다. 초점 거리(Focal Length), 시야각(Field of View), 조리개(Aperture), 초점 거리(Focus Distance), 피사계 심도(Depth of Field), 렌즈 왜곡(Lens Distortion), 노출 시간(Exposure Time), 셔터 동작(Shutter Behavior), 게인(Gain), 동적 범위(Dynamic Range), 센서 해상도(Sensor Resolution)는 최종 영상에 영향을 줄 수 있다. 완벽하게 렌더링된 환경이라도 이상적인 카메라를 통해 관측하면 실제 로봇 영상 하드웨어에서 생성되는 데이터와 상당히 다를 수 있다.

모션 블러(Motion Blur)는 노출 시간 동안 카메라 또는 관측 객체가 움직일 때 발생한다. 그 크기는 노출 시간과 영상에서의 상대 운동(Relative Image Motion)에 따라 결정된다. 롤링 셔터 카메라(Rolling-Shutter Camera)는 영상의 각 행이 서로 다른 시점에 촬영되기 때문에 추가적인 기하학적 왜곡을 발생시킨다. 레이 트레이싱 렌더링과 시간적 카메라 모델(Temporal Camera Model)을 결합하면 합성 영상에서 이러한 효과를 재현할 수 있으며, 이는 특징 추적(Feature Tracking), 시각 오도메트리(Visual Odometry), 객체 검출에 큰 영향을 줄 수 있다.

이미지 센서(Image Sensor)는 광학 렌더링 이후에도 여러 가지 불완전성을 추가한다. 광자 통계(Photon Statistics), 전자 읽기 노이즈(Electronic Read Noise), 암전류(Dark Current), 양자화(Quantization), 포화(Saturation), 색상 처리(Color Processing), 압축(Compression), 자동 노출(Auto Exposure)이 소프트웨어에 전달되는 영상을 변화시킬 수 있다. 필요한 충실도에 따라 이러한 효과를 렌더링 이후에 적용하거나 보다 상세한 카메라 파이프라인에 통합할 수 있다. 센서 수준의 불완전성을 포함하면 알고리즘이 비현실적으로 깨끗한 합성 영상만을 이용하여 학습하는 것을 방지할 수 있다.

높은 동적 범위(High Dynamic Range)는 매우 밝은 영역과 매우 어두운 영역이 동시에 존재하는 장면에서 중요하다. 실외 로봇은 직사광선, 반사 표면, 깊은 그림자를 동시에 관측할 수 있으며, 실내 로봇은 창문이나 강한 인공 조명을 접할 수 있다. 포토리얼리스틱 시뮬레이터는 카메라 노출과 톤 처리(Tone Processing)를 적용하기 전에 물리적으로 의미 있는 조명 정보를 유지해야 하며, 이를 통해 포화, 노출 부족(Underexposure), 적응(Adaptation)을 현실적으로 표현할 수 있다.

포토리얼리스틱 렌더링은 깊이 및 능동 광학 센서(Active Optical Sensor)를 지원할 수도 있지만 그 물리적 동작은 수동 RGB 카메라와 다르다. 스테레오 깊이(Stereo Depth)는 기하구조, 텍스처, 보정(Calibration), 대응점 탐색(Correspondence)에 의존하며, 구조광(Structured Light) 및 비행시간(Time of Flight, ToF) 시스템은 능동 조명과 센서별 아티팩트(Artifact)를 발생시킨다. RTX 기반 장면 기하구조와 재질 정보는 RGB 관측값과 함께 이러한 센서를 모델링하기 위한 기반을 제공할 수 있다.

가시광 렌더링과 관련된 재질 특성(Material Property)은 능동 센싱에서 다르게 영향을 줄 수도 있다. 시각적으로 밝게 보이는 표면이 적외선 파장(Infrared Wavelength)에서 동일한 반응을 보이지 않을 수 있으며, 투명하거나 반사성이 높은 객체는 깊이 센서에서 오류를 발생시킬 수 있다. 따라서 고충실도 시뮬레이션에서는 다중 모달 데이터(Multimodal Data)를 생성할 때 시각적 외관과 센서별 광학 반응을 구분해야 하며 하나의 재질 표현이 모든 센서에서 동일하게 동작한다고 가정해서는 안 된다.

포토리얼리스틱 시뮬레이션은 현실적인 센서 관측값과 함께 정확한 라벨을 생성할 수 있기 때문에 합성 데이터셋 생성(Synthetic Dataset Generation)에 특히 유용하다. RGB 영상에는 깊이, 의미론적 분할(Semantic Segmentation), 인스턴스 분할(Instance Segmentation), 표면 법선, 광학 흐름(Optical Flow), 객체 자세(Object Pose), 바운딩 박스(Bounding Box), 운동 정보(Motion Information)를 함께 제공할 수 있다. 이러한 주석(Annotation)은 가상 장면에서 직접 생성되므로 실제 데이터셋에서 필요한 상당한 수작업 라벨링을 줄일 수 있다.

도메인 랜덤화(Domain Randomization)는 생성되는 샘플마다 조명, 텍스처, 재질, 날씨, 객체 외관, 카메라 파라미터, 노출, 노이즈, 장면 구성을 변화시킬 수 있다. 포토리얼리즘과 랜덤화는 서로 배타적인 전략이 아니라 상호 보완적인 전략이다. 고충실도 렌더링은 물리적으로 그럴듯한 관측값을 제공하고, 제어된 변화는 학습 과정에서 경험하는 외관 분포를 확장하여 하나의 시뮬레이션 환경에 대한 의존도를 낮출 수 있다.

절차적 장면 생성(Procedural Scene Generation)은 객체 배치, 기하구조, 재질, 조명, 환경 조건을 자동으로 변경하여 데이터셋의 다양성을 더욱 높일 수 있다. RTX 렌더링과 결합하면 모든 구성을 수작업으로 제작하지 않고도 시각적으로 서로 다르면서 물리적으로 구조화된 대규모 장면을 생성할 수 있다. 이를 통해 검출, 분할, 깊이 추정, 자세 추정, 내비게이션, 체화 인지(Embodied Perception) 모델의 확장 가능한 학습을 지원할 수 있다.

렌더링 충실도(Rendering Fidelity)는 시뮬레이션 처리량(Simulation Throughput)과 균형을 이루어야 한다. 광선 깊이(Ray Depth), 픽셀당 샘플 수(Samples Per Pixel), 텍스처 해상도, 기하학적 복잡도, 전역 조명 품질을 높이면 일반적으로 계산 비용이 증가한다. 학습 파이프라인은 높은 프레임 처리량을 선호할 수 있지만 최종 검증에서는 최대한의 시각적 정확도가 필요할 수 있다. 따라서 실용적인 시스템은 여러 렌더링 품질 수준을 제공하고 각 실험의 목적에 따라 충실도를 선택할 수 있다.

디노이징(Denoising)은 한 프레임에서 평가할 수 있는 광선 수가 제한되는 실시간 또는 저샘플 레이 트레이싱에서 자주 사용된다. 공간 및 시간 디노이저(Spatial and Temporal Denoiser)는 깊이, 표면 법선, 모션 벡터(Motion Vector), 이전 프레임과 같은 정보를 이용하여 노이즈가 많은 레이 트레이싱 샘플에서 부드러운 영상을 재구성한다. 디노이징은 시각적 품질을 향상시키지만 아티팩트를 생성할 수도 있으므로 렌더링 영상이 머신러닝 입력으로 사용될 때 그 영향을 고려해야 한다.

검증(Validation)은 동일한 기하구조, 재질, 조명, 카메라 구성, 움직임 조건에서 시뮬레이션 센서 데이터와 실제 센서 데이터를 비교해야 한다. 평가 항목에는 영상 밝기 분포(Image Intensity Distribution), 색상 응답(Color Response), 모서리, 반사, 그림자, 노이즈, 깊이 동작(Depth Behavior), 특징 통계(Feature Statistics), 후속 인지 성능(Downstream Perception Performance)이 포함될 수 있다. 포토리얼리스틱한 외관만으로는 충분하지 않으며 시뮬레이션은 목표 로봇 알고리즘에 실질적인 영향을 주는 특성을 재현해야 한다.

실용적인 포토리얼리스틱 센서 렌더링 아키텍처(Photorealistic Sensor Rendering Architecture)는 장면 기하구조, 물리 기반 재질, 광원, 레이 트레이싱 또는 패스 트레이싱, RTX 가속, 카메라 광학, 노출, 센서 노이즈, 시간적 효과(Temporal Effect), 주석 생성, 도메인 랜덤화를 연결한다. 필요한 충실도는 인지 작업과 사용 가능한 계산 자원에 따라 결정된다. 적절하게 설계된 렌더링 시스템은 인지, 합성 학습, 검증, 시뮬레이션-현실 전이(Sim2Real Transfer)를 위한 확장 가능하고 현실적인 센서 데이터를 제공한다.
