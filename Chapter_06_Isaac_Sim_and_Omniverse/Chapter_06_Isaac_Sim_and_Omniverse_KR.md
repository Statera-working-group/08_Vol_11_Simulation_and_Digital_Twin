**Volume 11. Simulation and Digital Twin**

# Chapter 06. Isaac Sim and Omniverse

## 06.01. NVIDIA Omniverse and Isaac Sim Architecture Overview

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

NVIDIA Omniverse와 Isaac Sim은 물리 기반 시뮬레이션(physically based simulation), 3D 장면 표현(3D scene representation), 로봇 개발(robotics development), 합성 데이터 생성(synthetic data generation), AI 학습(AI training)을 하나의 공통된 워크플로우 안에서 연결하도록 설계된 시뮬레이션 중심 소프트웨어 생태계이다. Omniverse는 보다 광범위한 플랫폼 및 협업 기반을 제공하고, Isaac Sim은 물리(physics), 센서(sensors), 로봇 모델(robot models), ROS 2와의 연동을 중심으로 하는 로봇 시뮬레이션 기능을 제공한다. 두 시스템을 함께 사용하면 가상 로봇과 환경 모델링에서부터 검증, 합성 데이터 생성, 강화학습(reinforcement learning), 실제 로봇 배포(deployment)에 이르는 개발 프로세스를 지원할 수 있다.

아키텍처 수준에서 중요한 개념은 일반적인 3D 시뮬레이션 생태계와 로봇 특화 기능을 분리하는 것이다. Omniverse는 장면(scene), 에셋(asset), 렌더링(rendering), 상호운용성(interoperability)을 위한 기반을 제공하고, Isaac Sim은 여기에 로봇 시뮬레이션 기능인 관절형 로봇 모델(articulated robot models), 물리적 상호작용(physics interaction), 센서 시뮬레이션(sensor simulation), 로봇공학 인터페이스(robotics interfaces) 등을 추가한다. 이러한 분리를 통해 각각의 알고리즘이나 로봇 하위 시스템마다 별도의 독립적인 시뮬레이션을 구축하는 대신 동일한 가상 환경과 에셋을 다양한 개발 활동에서 재사용할 수 있다.

이 생태계의 핵심적인 표현 기술 중 하나가 Universal Scene Description(USD)이다. USD는 장면(scene), 객체(objects), 관계(relationships), 변환(transform), 재질(materials) 및 기타 속성을 구조화된 방식으로 표현하여 복잡한 시뮬레이션 환경을 재사용 가능한 구성 요소로 조립할 수 있도록 한다. 로봇공학에서는 하나의 장면 안에 로봇, 지형, 건물, 차량, 사람, 조명, 센서 및 의미론적 정보(semantic information)가 동시에 포함될 수 있기 때문에 이것이 특히 중요하다. 일관된 장면 표현을 사용하면 점점 복잡해지는 시뮬레이션 시나리오를 보다 쉽게 구성하고, 수정하고, 재사용하고, 관리할 수 있다.

Isaac Sim은 이러한 장면 기반 접근 방식에 로봇 중심의 시뮬레이션 기능을 추가한다. 로봇은 물리적 구조, 관절, 액추에이터, 충돌 형상(collision geometry) 및 관련 속성으로 표현될 수 있으며, 이를 통해 실제 물리적 상호작용을 고려하면서 로봇의 동작을 평가할 수 있다. 따라서 시뮬레이션은 단순한 시각적 렌더링을 넘어선다. 로봇의 움직임, 접촉(contact), 충돌(collision), 센서 관측값(sensor observations), 환경과의 상호작용이 실제 시스템을 동작시키는 소프트웨어 아키텍처와 동일한 흐름의 입력으로 사용될 수 있다. 이를 통해 로봇의 전체 생명주기에 걸쳐 시뮬레이션 기반 개발(simulation-driven development)을 수행할 수 있다.

물리 시뮬레이션(physics simulation)은 또 하나의 중요한 아키텍처 계층이다. 시뮬레이터는 강체(rigid body), 관절(joint), 접촉(contact), 힘(force) 및 운동(motion)이 시뮬레이션 시간에 따라 어떻게 변화하는지를 계산해야 한다. 이 계층의 품질은 가상 로봇이 실제 로봇과 얼마나 유사하게 동작하는지를 결정하는 데 직접적인 영향을 준다. 따라서 Isaac Sim을 사용할 때에도 앞서 설명한 시뮬레이션 기초가 중요하다. 시간 간격(time step)의 선택, 수치적 안정성(numerical stability), 강체 동역학(rigid-body dynamics), 접촉 모델링(contact modeling), 벤치마킹(benchmarking) 등이 모두 시뮬레이션 결과의 유용성에 영향을 미친다.

센서 시뮬레이션(sensor simulation)은 실제 물리 세계 모델을 인식(perception) 및 AI 소프트웨어와 연결하는 역할을 한다. 카메라는 영상 관측값을 생성할 수 있고, 깊이 센서(depth sensor), LiDAR, 레이더(radar), IMU, GNSS 및 기타 센서 역시 이에 대응하는 가상 측정값을 생성할 수 있다. 목적은 단순히 시각적으로 매력적인 장면을 만드는 것이 아니라, 노이즈(noise), 환경적 영향(environmental effects), 센서 타이밍(sensor timing), 기하학적 관계(geometric relationships) 등 알고리즘에 중요한 센서 특성을 재현하는 것이다. 특히 시각 인식 모델을 실제와 유사한 조건에서 학습하거나 평가해야 하는 경우에는 사실적인 렌더링(photorealistic rendering)과 RTX 기반 기술이 중요한 역할을 한다.

Isaac Sim과 로봇 소프트웨어 스택(robot software stack)의 연결은 Physical AI 개발에서 특히 중요하다. ROS 2 통합 계층을 사용하면 시뮬레이션된 센서가 실제 로봇에서 사용하는 것과 유사한 인터페이스를 통해 데이터를 발행하고, 시뮬레이션된 액추에이터는 명령을 수신할 수 있다. 이를 통해 인식(perception), 위치추정(localization), 경로계획(planning), 제어(control), 고수준 자율성(autonomy) 소프트웨어를 모든 물리 시스템을 준비하지 않고도 테스트할 수 있다. 따라서 동일한 아키텍처 인터페이스를 활용하여 시뮬레이션 테스트와 하드웨어 테스트 사이의 차이를 줄일 수 있다.

Isaac Sim은 시뮬레이션을 단순한 테스트 환경이 아니라 데이터 생성 시스템(data-generation system)으로 활용할 때에도 유용하다. NVIDIA Omniverse Replicator를 사용하면 장면, 객체, 조명, 카메라 구성 및 기타 매개변수를 체계적으로 변화시키면서 합성 데이터셋(synthetic dataset)과 그에 대응하는 어노테이션(annotation)을 생성할 수 있다. 이는 AI 시스템에서 특히 중요하다. 실제 세계에서 희귀한 사건을 수집하고 라벨링하는 작업은 많은 비용과 시간이 필요하거나 현실적으로 어려울 수 있기 때문이다. 합성 데이터는 시나리오의 다양성을 명시적으로 제어하면서 실제 데이터셋을 보완할 수 있다.

강화학습(reinforcement learning)은 또 다른 아키텍처 차원을 추가한다. Isaac Gym과 이후의 Isaac Lab 프레임워크는 정책 학습(policy learning)을 위해 대규모의 시뮬레이션 환경을 동시에 실행하는 접근 방식을 제공한다. 하나의 고비용 시뮬레이션을 반복적으로 실행하는 대신 여러 환경을 병렬로 평가할 수 있기 때문에, 서로 다른 초기 조건과 환경 설정에서 훨씬 많은 경험 데이터를 수집할 수 있다. 이러한 아키텍처는 반복적인 환경 상호작용을 통해 학습해야 하는 보행(locomotion), 조작(manipulation), 내비게이션(navigation) 및 기타 제어 문제에 특히 유용하다.

따라서 Physical AI 개발 파이프라인에서 전체 아키텍처는 모델(models), 환경(environments), 물리(physics), 센서(sensors), 로봇 소프트웨어(robot software), AI, 물리적 검증(physical validation)을 연결하는 하나의 흐름으로 이해할 수 있다. 로봇과 환경 모델이 가상 세계를 구성하고, 물리 엔진(physics engine)이 물리적 상호작용을 결정하며, 시뮬레이션 센서가 관측값을 생성한다. ROS 2 또는 이에 준하는 인터페이스는 이러한 관측값을 로봇 소프트웨어와 연결하고, AI 모델은 의사결정을 수행하거나 정책을 학습한다. 이후 시뮬레이션 결과를 실제 로봇의 동작과 비교함으로써 시뮬레이션, 테스트, 학습, 보정(calibration), Sim2Real 전환을 반복할 수 있는 기반을 구축할 수 있다.

이 아키텍처는 시뮬레이션 표현이 완전히 정적인 가상 모델로 남아 있는 것이 아니라 실제 로봇의 정보와 연결되는 디지털 트윈(digital twin)의 기반도 제공한다. 실제 로봇의 상태(state), 텔레메트리(telemetry), 센서 정보, 설정(configuration), 운영 데이터(operational data)를 대응하는 가상 표현과 연결할 수 있다. 이를 통해 시뮬레이션을 실제 로봇 배포 이전에만 사용하는 것이 아니라, 실제 운영 중에도 시각화(visualization), 분석(analysis), 예측 실험(predictive experiments), 가상 시나리오 평가(what-if evaluation)에 활용할 수 있다. 이후의 디지털 트윈 장에서는 동기화(synchronization), 상태 미러링(state mirroring), 시각화, 예측 시뮬레이션(predictive simulation), 플릿 수준 집계(fleet-level aggregation) 등을 통해 이 개념을 확장한다.

Omniverse와 Isaac Sim의 실질적인 가치는 하나의 특정 시뮬레이션 기능에 있는 것이 아니라 여러 개발 단계를 하나의 공통 생태계로 통합할 수 있다는 점에 있다. 로봇 모델링(robot modeling), 환경 구축(environment construction), 물리 시뮬레이션, 센서 생성(sensor generation), ROS 2 통합, 합성 데이터 생성, 강화학습, 디지털 트윈 워크플로우를 하나의 연결된 개발 프로세스의 구성 요소로 다룰 수 있다. AMR, 사족보행 로봇(quadruped), 휴머노이드(humanoid), 매니퓰레이터(manipulator), UAV와 같은 다양한 로봇 프로그램에서 이러한 아키텍처를 활용하면 시뮬레이션의 충실도(fidelity)를 단계적으로 높이면서도 동일한 핵심 목표를 유지할 수 있다. 즉, 실제 테스트에 대한 의존성을 줄이는 동시에 실제 배포 이전에 확보할 수 있는 검증 및 근거의 양과 품질을 높이는 것이다.

## 06.02. Isaac Sim Installation and Workspace Setup [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

Isaac Sim 설치는 단일 데스크톱 애플리케이션을 설치하는 작업이라기보다 완전한 로봇 시뮬레이션 개발 환경(robotics simulation development environment)을 구축하는 과정으로 이해해야 한다. 이 환경은 NVIDIA GPU 가속(GPU acceleration), 시뮬레이션 런타임 구성 요소(simulation runtime components), USD 기반 장면 에셋(scene assets), 물리 및 렌더링 서비스(physics and rendering services), Python API, 로봇공학 확장 기능(robotics extensions), ROS 2와 같은 외부 미들웨어(external middleware)를 결합한다. 따라서 안정적인 구축은 시뮬레이션 워크로드를 실행할 전체 소프트웨어 및 하드웨어 스택(software and hardware stack)을 함께 고려하는 것에서 시작한다.

호스트 워크스테이션(host workstation)은 먼저 목표로 하는 시뮬레이션 규모에 충분한 GPU, CPU, 메모리(memory), 저장공간(storage)을 제공해야 한다. 특히 Isaac Sim은 물리 계산(physics computation)과 RTX 기반 렌더링(RTX-based rendering), 그리고 잠재적으로 여러 개의 시뮬레이션 센서를 함께 처리하므로 GPU 성능이 중요하다. 고해상도 카메라, LiDAR, 관절형 로봇(articulated robots), 상세한 에셋을 포함하는 복잡한 환경은 상당한 GPU 메모리를 사용할 수 있다. 또한 USD 장면, 텍스처(texture), 로봇 모델, 합성 데이터셋(synthetic datasets), 체크포인트(checkpoints), 실험 결과가 빠르게 증가할 수 있으므로 저장공간 계획도 중요하다.

소프트웨어 환경(software environment)은 제어 가능한 의존성 스택(dependency stack)으로 준비해야 한다. 운영체제(operating system), NVIDIA 그래픽 드라이버(graphics driver), GPU 런타임(runtime), Isaac Sim 릴리스(release), Python 환경, 선택적으로 사용하는 ROS 2 배포판(distribution)이 서로 호환되어야 한다. 버전 간 관계를 고려하지 않고 구성 요소를 개별적으로 설치하면 원래 의존성 문제와 직접적인 관련이 없어 보이는 오류가 발생할 수 있다. 따라서 설치 과정에서는 임의로 변경되어 온 워크스테이션 환경에 의존하기보다 소프트웨어 버전과 환경 변수(environment variables)를 문서화한 재현 가능한 구성(reproducible configuration)을 유지해야 한다.

Isaac Sim은 시뮬레이터 설치 영역과 사용자 프로젝트 작업공간(workspace)을 논리적으로 분리하여 구성할 수 있다. 시뮬레이터 영역에는 런타임 라이브러리(runtime libraries), 기본 확장 기능(built-in extensions), 렌더링 구성 요소, 물리 서비스(physics services), 표준 에셋이 포함되며, 작업공간에는 프로젝트별 로봇 모델, 환경, 스크립트, 설정 파일(configuration files), 실험 및 생성된 결과를 배치한다. 이러한 분리는 시뮬레이터 파일의 의도하지 않은 변경을 줄이고 프로젝트를 다른 워크스테이션, GPU 서버 또는 향후 Isaac Sim 릴리스로 이전하기 쉽게 만든다.

실용적인 작업공간은 여러 개의 지속적으로 관리되는 리소스 범주(resource categories)를 중심으로 구성할 수 있다. 로봇 에셋(robot assets)에는 USD, URDF, 메시(meshes), 재질(materials), 액추에이터 매개변수(actuator parameters), 센서 설정(sensor configurations)을 포함할 수 있다. 환경 에셋(environment assets)에는 공장, 창고, 야외 지형, 도로, 장애물, 조명 설정을 포함할 수 있다. 소스 디렉터리(source directories)에는 Python 시뮬레이션 스크립트와 확장 기능을 배치하고, 설정 디렉터리(configuration directories)에는 실험 매개변수를 보존할 수 있다. 로그, 데이터셋, 녹화 데이터, 체크포인트 및 평가 결과는 별도의 출력 디렉터리(output directories)에 저장하여 생성된 결과물이 소스 에셋과 혼합되지 않도록 관리할 수 있다.

USD는 로봇과 환경 장면을 반복적으로 복제하는 대신 조합(composition)할 수 있기 때문에 이러한 작업공간에서 중요한 구성 관리 수단이 된다. 하나의 기본 로봇 에셋(base robot asset)을 여러 실험에서 참조할 수 있으며, 센서, 페이로드(payload), 재질 또는 환경 객체를 장면 조합(scene composition)을 통해 추가할 수 있다. 이러한 접근 방식은 재사용 가능한 시뮬레이션 에셋을 지원하고, 개발팀이 통제된 원본 표현(source representation)을 유지하면서 개별 실험마다 특화된 장면 구성을 생성할 수 있도록 한다. 따라서 본 권의 다음 절에서는 USD를 로봇 장면(robot scenes)을 구성하기 위한 독립적인 기반 기술로 다룬다.

설치 후에는 복잡한 로봇 프로젝트를 적용하기 전에 먼저 시뮬레이터 자체를 검증해야 한다. 최소 장면(minimal scene)을 이용하여 애플리케이션 시작, GPU 렌더링, 물리 초기화(physics initialization), 타임라인 실행(timeline execution), 기본적인 객체 상호작용(object interaction)을 확인할 수 있다. 중력에 의해 낙하하거나 지면과 충돌하는 간단한 강체(rigid body)는 초기 물리 검증에 유용하다. 이러한 단계적 검증(staged validation)을 수행하면 설치 자체의 문제와 이후 가져온 메시, 로봇 기술(robot descriptions), 센서, 컨트롤러(controller), 미들웨어에서 발생하는 오류를 구분할 수 있다.

Python 실행 환경(Python execution environment)도 독립적으로 검증해야 한다. 자동화(automation)는 실제 Isaac Sim 워크플로우의 핵심이기 때문이다. 최소한의 스크립트를 통해 시뮬레이션을 초기화하고, 스테이지(stage)를 생성하거나 불러오며, 객체를 삽입하고, 시뮬레이션 스텝(simulation steps)을 진행하며, 상태를 확인한 후 정상적으로 종료할 수 있어야 한다. 이러한 기본 실행 경로가 정상적으로 동작하면 전적으로 수동 그래픽 조작에 의존하지 않고 스크립트를 통해 보다 큰 규모의 실험을 구현할 수 있다. 스크립트 기반 실행(script-based execution)은 재현성을 향상시키며 배치 시뮬레이션(batch simulation)과 대규모 실험에서도 필수적이다.

확장 기능(extensions)은 작업공간 아키텍처(workspace architecture)의 또 다른 중요한 부분이다. Isaac Sim의 기능은 로봇공학 유틸리티(robotics utilities), 센서, 인터페이스, 시각화(visualization), 특정 워크플로우 도구 등을 제공하는 모듈형 서비스(modular services)와 확장 기능으로 구성된다. 프로젝트에서는 목표 워크로드에 필요한 구성 요소만 활성화하고 중요한 확장 기능 의존성을 기록하는 것이 바람직하다. 이후 사용자 정의 확장 기능(custom extensions)을 이용하면 조직 또는 프로젝트에 특화된 기능을 시뮬레이터 핵심부와 분리할 수 있으며, 기본 설치 환경을 직접 수정하지 않고도 재사용 가능한 도구나 로봇별 기능을 발전시킬 수 있다.

ROS 2 통합(integration)은 독립적인 시뮬레이터와 Python 환경이 정상적으로 동작하는 것을 확인한 이후 구성하는 것이 적절하다. ROS 2 브리지(ROS 2 bridge)는 시뮬레이션 센서, 로봇 상태(robot states), 명령(commands), 로봇 소프트웨어를 ROS 호환 통신 인터페이스(ROS-compatible communication interfaces)를 통해 연결한다. 시뮬레이터와 ROS 2 프로세스 사이에서 환경 설정, 메시지 정의(message definitions), 도메인 설정(domain settings), 토픽 규칙(topic conventions)이 일관되게 유지되어야 한다. 브리지를 별도로 검증하면 통신 오류가 물리, 렌더링 또는 로봇 모델 문제와 혼동되는 것을 방지할 수 있다.

로봇 가져오기(robot import)는 작업공간 검증의 다음 단계이다. URDF 또는 다른 로봇 기술(robot description)을 가져온 후 자율 소프트웨어를 연결하기 전에 링크 계층구조(link hierarchy), 관절 축(joint axes), 제한값(limits), 관성 특성(inertial properties), 충돌 형상(collision geometry), 액추에이터 동작(actuator behavior)을 확인해야 한다. 시각적으로 올바르게 보이는 것만으로는 충분하지 않으며, 외형상 정상적인 로봇이라도 비현실적인 질량 분포(mass distribution)나 충돌 모델을 포함할 수 있다. 시뮬레이션 품질은 앞서 다룬 관성 계산(inertia calculation), 메시 최적화(mesh optimization), 액추에이터 모델링(actuator modeling), 이동 로봇 구성(mobile robot configuration)과 같은 로봇 모델링 원칙에 크게 의존한다.

센서 설정(sensor setup) 역시 동일한 단계적 접근 방식을 따라야 한다. 먼저 카메라를 통해 렌더링과 영상 획득(image acquisition)을 검증한 후 깊이 센서, LiDAR, IMU, 레이더 및 기타 센서를 추가할 수 있다. 좌표 프레임(coordinate frames), 장착 변환(mounting transforms), 갱신 주기(update rates), 타임스탬프(timestamps), 해상도(resolution), 노이즈 특성(noise characteristics), 데이터 인터페이스(data interfaces)를 각각 확인해야 한다. 센서 시뮬레이션은 단순한 시각화 기능이 아니며, 센서 출력은 궁극적으로 인식(perception), 위치추정(localization), 매핑(mapping), 경로계획(planning), AI 파이프라인의 입력이 되기 때문에 이러한 검증 과정이 중요하다.

작업공간 구성에서는 대화형 개발(interactive development)과 자동화 실행(automated execution)도 구분해야 한다. 장면을 구성하고 디버깅(debugging)하는 동안에는 그래픽 환경을 이용하여 형상, 변환, 충돌, 센서 및 로봇 동작을 직접 확인하는 것이 유용하다. 실험이 안정화된 이후에는 회귀 테스트(regression testing), 데이터셋 생성, 강화학습(reinforcement learning), 시뮬레이션 팜(simulation farms)을 위해 스크립트 기반 또는 헤드리스 실행(headless execution)이 보다 적합하다. 두 가지 실행 방식을 함께 유지하면 실험 아키텍처를 다시 구축하지 않고도 시각적 디버깅에서 확장 가능한 자동화로 자연스럽게 전환할 수 있다.

AI 중심 프로젝트에서는 생성된 데이터를 시뮬레이터 디렉터리 내부의 임의 위치에 저장하는 것이 아니라 관리되는 출력(managed output)으로 취급해야 한다. 합성 RGB 영상, 깊이 맵(depth maps), 세그멘테이션 라벨(segmentation labels), 포인트 클라우드(point clouds), 궤적(trajectories), 시연 데이터(demonstrations), 강화학습 체크포인트(reinforcement-learning checkpoints), 평가 지표(evaluation metrics)는 시뮬레이션 소스 자체보다 훨씬 큰 용량으로 증가할 수 있다. 따라서 결과가 생성된 정확한 시뮬레이션 조건을 추적할 수 있도록 데이터셋 위치, 실험 식별자(experiment identifiers), 랜덤 시드(random seeds), 설정 스냅샷(configuration snapshots), 모델 버전을 기록해야 한다.

성숙한 작업공간은 최종적으로 로컬 워크스테이션(local workstations), 온프레미스 GPU 서버(on-premise GPU servers), 대규모 시뮬레이션 인프라(simulation infrastructure) 사이에서 재현성(reproducibility)을 지원해야 한다. 소스 코드, 설정 파일, 경량 장면 정의(lightweight scene definitions), 메타데이터(metadata)는 버전 관리(version control)를 적용하고, 대용량 에셋과 데이터셋은 별도로 관리할 수 있다. 설치 검증 스크립트와 표준화된 프로젝트 구조는 특정 장비에 대한 의존성을 줄여준다. 이러한 특성은 Isaac Sim이 개별 개발 환경에서 Isaac Lab 학습, 합성 데이터 파이프라인, 디지털 트윈(digital twins), 대규모 병렬 시뮬레이션(massively parallel simulation)으로 확장될수록 더욱 중요해진다.

따라서 완료된 설치 환경은 단순히 애플리케이션 실행에 성공한 상태가 아니라 검증된 시뮬레이션 플랫폼(validated simulation platform)으로 이해해야 한다. GPU 렌더링, 물리, Python 실행, USD 에셋, 로봇 모델, 센서, 확장 기능, ROS 2 인터페이스, 출력 관리(output management)를 각각 단계적으로 검증해야 한다. 이러한 기반이 안정화되면 동일한 작업공간을 단순한 로봇 실험에서 합성 데이터 생성, 강화학습, Sim2Real 검증, 디지털 트윈 운영, 대규모 병렬 시뮬레이션으로 확장할 수 있으며, 개발 환경을 매번 처음부터 다시 구축할 필요가 없어진다.

## 06.03. USD Universal Scene Description for Robot Scenes [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

범용 장면 기술(Universal Scene Description), 일반적으로 USD라고 줄여 부르는 기술은 NVIDIA Omniverse와 Isaac Sim에서 복잡한 가상 환경(virtual environments)을 구성하고, 조직하고, 교환하고, 수정하기 위한 장면 표현 기반(scene representation foundation)을 제공한다. USD는 로봇 시뮬레이션을 하나의 거대한 단일 모델 파일(monolithic model file)로 처리하는 대신, 장면을 서로 연결된 요소들의 계층구조(hierarchy)로 표현한다. 따라서 로봇, 센서, 건물, 지형, 재질(materials), 조명, 동적 객체(dynamic objects)를 각각 독립적인 에셋(asset)으로 유지하면서 하나의 통합된 시뮬레이션 장면에 참여시킬 수 있다.

USD 장면의 기본 단위는 프림(prim), 즉 프리미티브(primitive)이다. 프림은 장면 계층구조 안에서 식별 가능한 객체 또는 구성 요소를 나타내며, 기하 형상(geometry), 변환(transform), 카메라, 조명, 재질 또는 다른 구조화된 개체(structured entity)를 표현할 수 있다. 프림은 경로(path)를 통해 구성되며, 이를 통해 장면 그래프(scene graph)가 형성된다. 로봇 시뮬레이션에서는 이러한 계층구조를 이용하여 바퀴, 센서, 페이로드 모듈(payload modules), 매니퓰레이터(manipulators)를 포함하는 이동 베이스(mobile base)와 같은 조립체를 자연스럽게 표현할 수 있다.

USD는 장면 구성(scene organization)을 객체를 시각화하는 데 사용되는 원시 기하 형상(raw geometry)과 분리한다. 로봇 링크(robot link)는 메시(mesh)를 참조하면서도 자체적인 변환, 물리적 속성(physical properties), 재질 할당(material assignments), 다른 장면 요소와의 관계를 유지할 수 있다. 이러한 분리 방식에서는 동일한 기하 에셋(geometric asset)을 전체 데이터를 복사하지 않고 여러 로봇이나 환경에서 재사용할 수 있다. 따라서 대규모 시뮬레이션 프로젝트는 재사용 가능한 구성 요소 라이브러리를 유지하면서 개별 장면 기술(scene description)을 비교적 간결하게 관리할 수 있다.

USD에서 가장 중요한 개념 중 하나는 구성(composition)이다. 모든 구성 요소를 하나의 파일로 영구적으로 병합하는 대신 여러 계층(layer)과 에셋을 조합하여 하나의 장면을 구성할 수 있다. 참조(reference)를 이용하면 기존 에셋을 다른 장면 내부에서 사용할 수 있으며, 페이로드(payload)는 복잡한 콘텐츠를 선택적으로 불러오는 데 활용할 수 있다. 서브레이어(sublayer)는 독립적으로 작성된 장면 정보를 결합하고, 변형(variant)은 서로 다른 구성을 표현할 수 있다. 이러한 메커니즘을 함께 활용하면 원본 에셋을 훼손하지 않고 시뮬레이션 장면을 지속적으로 발전시킬 수 있다.

계층화(layering)는 서로 다른 엔지니어링 관심 영역(engineering concerns)을 독립적으로 관리할 수 있기 때문에 로봇 개발에서 특히 유용하다. 기본 계층(base layer)은 로봇의 기하 형상을 정의하고, 다른 계층에서는 물리적 속성을 추가하며, 또 다른 계층에서는 센서를 구성할 수 있다. 실험별 계층(experiment-specific layer)에서는 초기 자세(initial poses)나 환경 조건을 변경할 수 있다. 따라서 엔지니어는 상위 원본 에셋(upstream assets)을 유지하면서 특정 실험에 필요한 정보만 수정할 수 있으며, 이러한 비파괴적 워크플로우(non-destructive workflow)는 반복적인 시뮬레이션, 보정(calibration), 검증(validation)에 적합하다.

변형(variant)은 로봇 구성을 관리하기 위한 또 하나의 강력한 메커니즘이다. 하나의 로봇 에셋에서도 서로 다른 센서 패키지(sensor packages), 휠 구성(wheel configurations), 페이로드, 재질 또는 기하 형상의 상세도(level of geometric detail)가 필요할 수 있다. 모든 조합마다 완전한 별도의 모델을 유지하는 대신 변형 집합(variant sets)을 이용하여 동일한 에셋 구조 안에서 선택 가능한 대안을 표현할 수 있다. 이를 통해 중복 데이터를 줄이고 서로 연관된 로봇 구성이 공통된 기반을 지속적으로 공유하도록 관리할 수 있다.

변환(transform)은 객체가 장면 계층구조에서 부모 객체(parent)에 대해 어떤 위치와 방향을 갖는지를 정의한다. 로봇공학에서는 센서 측정값, 충돌 형상(collision geometry), 관절 구조(joint structures), 제어 인터페이스(control interfaces)가 정확한 공간적 관계(spatial relationships)에 의존하기 때문에 변환의 일관성이 매우 중요하다. 예를 들어 이동 로봇에 장착된 카메라는 로봇 본체에 대해 의도된 병진(translation)과 회전(rotation)을 유지해야 한다. 변환 계층구조의 오류는 인식(perception), 위치추정(localization), 매핑(mapping), 센서 융합(sensor fusion)의 결과까지 전파될 수 있다.

로봇 장면(robot scenes)은 시각적 기하 형상을 넘어 물리적 의미 정보(physical semantics)도 필요로 한다. 충돌 형상(collision shapes), 강체 속성(rigid-body properties), 질량(mass), 관성(inertia), 관절(joints), 관절체 구조(articulation structures), 액추에이터 관련 정보는 시뮬레이션된 로봇이 환경과 어떻게 상호작용하는지를 결정한다. Isaac Sim에서 사용되는 USD 기반 장면은 이러한 시뮬레이션 속성을 장면 객체와 연결하면서 전체 장면 계층구조를 유지할 수 있다. 이를 통해 앞서 다룬 URDF, MJCF, 관성, 메시, 액추에이터, 이동 로봇 구조의 모델링 개념을 Isaac Sim 환경과 연결할 수 있다.

시각적 기하 형상(visual geometry)과 충돌 기하 형상(collision geometry)은 반드시 동일할 필요가 없다. 상세한 메시는 렌더링(rendering)에는 적합하지만 반복적인 충돌 계산에는 불필요하게 많은 연산을 요구하거나 수치적으로 불안정할 수 있다. 따라서 로봇 에셋은 상세한 시각적 표현을 유지하면서 물리 계산에는 단순화된 충돌 근사 형상(collision approximations)을 사용할 수 있다. 장면에 다수의 로봇, 관절 메커니즘 또는 환경 객체가 포함될수록 물리 시뮬레이션 성능이 충돌 표현의 복잡도에 크게 영향을 받기 때문에 이러한 분리는 더욱 중요해진다.

재질(materials)과 외형 정보(appearance information)는 장면 표현의 또 다른 계층을 구성한다. 표면 외형(surface appearance)에는 사실적인 렌더링에 필요한 속성이 포함될 수 있으며, 시뮬레이션 애플리케이션에서는 객체에 추가적인 의미론적 또는 물리적 정보(semantic or physical information)를 연결할 수 있다. 합성 데이터 생성(synthetic data generation)에서는 텍스처(textures), 반사 특성(reflectance), 조명, 객체의 외형이 카메라 관측값에 영향을 주기 때문에 표면의 시각적 특성이 특히 중요하다. USD를 사용하면 이러한 속성을 매 실험마다 다시 생성하지 않고 재사용 가능한 장면 에셋과 연결하여 유지할 수 있다.

센서 역시 로봇 장면 계층구조의 구성 요소로 조직할 수 있다. 카메라, LiDAR, 레이더(radar), IMU 및 기타 시뮬레이션 센서는 로봇 본체와의 관계를 정의하는 장착 변환(mounting transforms)과 설정 매개변수(configuration parameters)를 필요로 한다. 센서를 구조화된 장면 표현 안에 배치하면 센서 구성을 보다 쉽게 확인하고, 재사용하고, 수정할 수 있다. 이는 이후 다루게 될 ROS 2 통합(integration)과 RTX 센서 시뮬레이션(RTX sensor simulation)을 위한 기반이 된다.

USD는 환경 모델링(environment modeling)에서도 동일하게 중요하다. 창고 장면은 선반, 팔레트, 문, 기계, 조명 시스템, 내비게이션 장애물을 각각 독립적인 에셋으로 참조할 수 있으며, 야외 환경은 지형, 도로, 식생(vegetation), 건물, 차량, 인프라 요소를 조합할 수 있다. 재사용 가능한 에셋 구성(asset composition)을 활용하면 환경을 서로 고립된 단일 세계로 모델링하는 대신 체계적으로 조립할 수 있다. 이러한 방식은 앞서 정의한 실내(indoor), 야외(outdoor), 항공(aerial), 절차적(procedural), 의미론적(semantic) 환경 워크플로우를 지원한다.

장면 인스턴싱(scene instancing)은 동일한 객체가 여러 번 등장하는 경우 중요해진다. 창고에는 수백 개의 유사한 팔레트나 저장 랙(storage racks)이 존재할 수 있으며, 야외 장면에서도 방호벽, 식생, 표지판, 인프라 요소가 반복적으로 등장할 수 있다. 반복되는 객체를 공유 에셋(shared assets)을 통해 표현하면 불필요한 데이터 중복을 줄이고 장면 관리를 효율화할 수 있다. 동일한 원칙은 다중 로봇 시뮬레이션(multi-robot simulation)에도 적용되며, 공통 로봇 플랫폼의 여러 인스턴스가 각각 독립적인 위치와 상태를 유지하면서 하나의 환경에서 동작할 수 있다.

따라서 잘 설계된 USD 계층구조는 단순히 가져온 CAD 모델의 구조를 그대로 복제하는 것이 아니라 의미 있는 에셋 경계(asset boundaries)를 반영해야 한다. 로봇 본체 조립체, 센서, 매니퓰레이터, 페이로드, 환경 모듈(environment modules), 재사용 가능한 인프라에는 명확한 소유 관계와 명명 규칙(naming conventions)이 필요하다. 안정적인 경로(stable paths)와 예측 가능한 계층구조는 Python 스크립트, 확장 기능(extensions), 합성 데이터 도구 또는 ROS 인터페이스가 특정 객체를 자동으로 탐색하고 조작해야 할 때 특히 중요하다.

USD는 수동으로 조립된 장면에서 프로그래밍 가능한 시뮬레이션(programmable simulation)으로 전환하는 과정도 지원한다. Python 기반 워크플로우에서는 프림을 생성하고, 에셋을 불러오며, 변환을 수정하고, 변형을 선택하며, 장면 속성을 설정하고, 실험별 환경을 생성할 수 있다. 이를 통해 장면 기술은 정적인 시각화 결과물이 아니라 자동화된 시뮬레이션 파이프라인(automated simulation pipeline)의 일부가 된다. 따라서 대규모 실험 집합에서도 공통된 원본 에셋을 공유하면서 스크립트를 이용해 테스트, 강화학습(reinforcement learning), 합성 데이터 생성을 위한 통제된 변형을 생성할 수 있다.

Physical AI 개발 관점에서 USD의 보다 중요한 의미는 시뮬레이션 생명주기(simulation lifecycle)의 여러 단계를 연결하는 공통 장면 표현(common scene representation)을 제공한다는 것이다. 로봇 모델, 환경, 물리 속성, 센서, 렌더링 정보, 의미론적 메타데이터(semantic metadata), 실험 설정을 조합 가능한 장면 구조 안에서 함께 관리할 수 있다. 이렇게 구성된 장면은 각각의 워크플로우마다 완전히 별도의 표현을 만들지 않고도 Isaac Sim 물리 시뮬레이션, ROS 2 인터페이스, RTX 센서, 합성 데이터 생성, 강화학습, 디지털 트윈(digital twins), 그리고 최종적인 Sim2Real 검증까지 지원할 수 있다.

## 06.04. Isaac Sim ROS2 Bridge Integration [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

ROS 2 브리지 통합(ROS 2 Bridge integration)은 NVIDIA Isaac Sim이 실제 로봇에서 일반적으로 사용하는 것과 동일한 메시지 중심 인터페이스(message-oriented interfaces)를 통해 로봇 소프트웨어 스택(robotics software stack)과 통신할 수 있도록 한다. 시뮬레이션 전용의 별도 인식(perception), 내비게이션(navigation), 경로계획(planning), 제어(control) 소프트웨어를 개발하는 대신, 개발자는 ROS 2 노드(nodes)를 시뮬레이션된 로봇과 센서에 연결할 수 있다. 이때 Isaac Sim은 가상의 물리 시스템(virtual physical system)으로 동작하고, ROS 2는 이를 둘러싼 분산 소프트웨어 통신 계층(distributed software communication layer)을 제공한다.

Isaac Sim과 ROS 2 사이의 아키텍처 경계(architectural boundary)는 중요하다. Isaac Sim은 로봇 동역학(robot dynamics), 충돌(collisions), 관절(joints), 액추에이터(actuators), 카메라, LiDAR, IMU 및 기타 가상 장치를 포함하는 시뮬레이션 세계를 담당한다. ROS 2 노드는 시뮬레이션 외부 또는 시뮬레이션과 함께 실행되면서 관측값을 수신하고, 상태를 추정하고, 계획을 생성하거나 명령을 출력한다. 브리지(bridge)는 이러한 두 영역 사이의 정보를 변환하여 시뮬레이션 데이터가 일반적인 ROS 2 계산 그래프(computation graph)에 참여할 수 있도록 한다.

통신은 기본적으로 양방향(bidirectional)이다. 시뮬레이션에서 ROS 방향으로 Isaac Sim은 센서 관측값(sensor observations), 로봇 상태(robot state), 관절 정보(joint information), 변환 정보(transforms), 클록 정보(clock information) 및 기타 시뮬레이션 측정값을 제공할 수 있다. 반대로 ROS에서 시뮬레이션 방향으로 ROS 2 소프트웨어는 속도 명령(velocity commands), 관절 명령(joint commands), 궤적(trajectories) 또는 상위 수준의 제어 요청을 전달할 수 있다. 이러한 폐루프 통신(closed communication loop)을 통해 시뮬레이션 로봇은 이후 실제 하드웨어와 상호작용하게 될 소프트웨어 로직과 본질적으로 동일한 로직에 반응할 수 있다.

토픽(topics)은 이러한 통합에서 가장 일반적인 ROS 2 통신 메커니즘을 제공한다. 시뮬레이션 카메라는 영상 데이터를 발행할 수 있고, LiDAR 시스템은 포인트 클라우드(point cloud) 또는 스캔 정보를 제공할 수 있으며, 로봇 구성 요소는 관절 상태(joint states)나 오도메트리 상태(odometry states)를 제공할 수 있다. 제어 노드는 반대 방향으로 명령 메시지를 발행할 수 있다. 안정적인 토픽 이름(topic names)과 메시지 형식(message types)을 유지하는 것은 애플리케이션 소프트웨어가 최소한의 아키텍처 변경만으로 시뮬레이션 데이터와 실제 데이터 소스를 전환할 수 있도록 한다는 점에서 중요하다.

좌표 프레임(coordinate frames) 역시 중요하다. 로봇 소프트웨어는 거의 모든 측정값을 공간적으로 해석하기 때문이다. ROS 2는 일반적으로 월드(world), 맵(map), 오도메트리(odometry), 로봇 베이스(robot base), 센서 및 관절 링크(articulated links) 등의 프레임을 연결하는 변환 트리(transform tree)를 통해 이러한 관계를 구성한다. Isaac Sim은 USD를 통해 자체적인 장면 계층구조(scene hierarchy)와 변환 정보를 가진다. 따라서 통합 과정에서는 시뮬레이션 장면 계층구조와 ROS 2 소프트웨어가 요구하는 좌표 프레임 규칙(coordinate-frame conventions) 사이의 일관된 매핑(mapping)이 필요하다.

이동 로봇(mobile robot)은 이러한 요구사항을 명확하게 보여준다. 시뮬레이션에는 월드 프레임(world frame), 섀시(chassis), 바퀴, 카메라, LiDAR, IMU, 매니퓰레이터(manipulator)가 서로 연관된 USD 객체로 존재할 수 있다. 동시에 ROS 2 스택은 map, odom, base_link, camera, lidar 등의 프레임을 요구할 수 있다. 이러한 관계는 동일한 물리적 배치를 표현해야 한다. 잘못된 프레임 방향(frame orientation)이나 센서 장착 오프셋(sensor mounting offsets)은 실제 알고리즘이 올바르더라도 위치추정(localization), 매핑(mapping), 인식(perception), 센서 융합(sensor fusion)에서 발생한 문제처럼 보이는 오류를 만들어낼 수 있다.

시뮬레이션 시간(simulation time) 역시 명시적으로 처리해야 한다. Isaac Sim은 실시간(real time), 실시간보다 느린 속도, 실시간보다 빠른 속도 또는 제어된 스텝(controlled stepping) 방식으로 동작할 수 있는 시뮬레이션 타임라인(simulation timeline)에 따라 진행된다. 타임스탬프(timestamps), 타이머(timers), 변환, 동기화(synchronization), 타임아웃 로직(timeout logic)을 사용하는 ROS 2 알고리즘은 적절한 시뮬레이션 클록(simulation clock)을 기준으로 동작해야 한다. 일관된 시간 의미 체계(time semantics)는 시뮬레이션 센서 메시지가 관련 없는 호스트 시스템 클록(host-system clock)을 기준으로 해석되는 문제를 방지한다.

여러 개의 시뮬레이션 장치가 동일한 인식 파이프라인(perception pipeline)에 참여하면 센서 동기화(sensor synchronization)가 특히 중요해진다. 카메라 영상, LiDAR 스캔, IMU 측정값, 깊이 정보(depth information), 로봇 상태는 서로 다른 갱신 주기(update rates)로 동작할 수 있다. 브리지는 ROS 2 노드가 측정값을 올바르게 연관시킬 수 있도록 의미 있는 타임스탬프를 유지해야 한다. 이는 본 권의 센서 시뮬레이션 부분에서 소개한 센서 동기화 및 타임스탬프 관리(sensor synchronization and timestamp management)에 대한 보다 광범위한 시뮬레이션 요구사항으로 연결된다.

ROS 2 서비스 품질(Quality of Service, QoS)은 통합 과정에서 추가적으로 고려해야 하는 요소이다. 높은 전송률의 센서 스트림(high-rate sensor streams)과 낮은 전송률의 설정 또는 상태 메시지는 반드시 동일한 전달 동작(delivery behavior)을 필요로 하지 않는다. 신뢰성(reliability), 내구성(durability), 큐 깊이(queue depth), 히스토리 정책(history policies)은 발행자(publisher)와 구독자(subscriber)가 데이터를 교환하는 방식에 영향을 준다. 양쪽 엔드포인트(endpoint)의 설정이 일치하지 않으면 정상적인 시뮬레이션 센서가 사용할 수 없는 것처럼 보일 수 있다. 따라서 브리지 검증에서는 토픽 이름뿐만 아니라 메시지 정의와 통신 정책도 함께 확인해야 한다.

제어 경로(control path)는 인식 경로(perception path)와 독립적으로 검증하는 것이 좋다. 이동 로봇에서는 간단한 속도 명령을 이용하여 ROS 2 메시지가 시뮬레이션 컨트롤러(simulated controller)에 전달되고 예상된 바퀴 또는 조향 동작을 생성하는지 먼저 확인할 수 있다. 매니퓰레이터에서는 관절 위치(joint position), 속도(velocity), 토크 또는 힘(effort), 궤적 명령(trajectory commands)을 이용하여 액추에이터 인터페이스를 검증할 수 있다. 기본적인 명령 실행이 안정적으로 동작한 이후에 내비게이션, 조작 계획(manipulation planning), 행동 트리(behavior trees), 자율 임무 로직(autonomous mission logic)을 추가하는 것이 적절하다.

센서 경로(sensor path) 역시 단계적으로 도입할 수 있다. 먼저 카메라 스트림(camera stream)을 통해 영상 전송을 검증한 후 LiDAR, 깊이 센서, IMU 또는 기타 센서를 연결할 수 있다. 각 스트림에서는 프레임 식별자(frame identity), 타임스탬프, 갱신 주기, 해상도(resolution), 데이터 형식(data format), 물리적 장착 설정(physical mounting configuration)을 확인해야 한다. 이러한 단계적 접근 방식은 브리지 설정 오류를 시뮬레이션 센서 모델 자체의 문제와 분리할 수 있도록 한다. 센서 모델의 충실도(fidelity)와 노이즈 특성(noise characteristics)은 별도로 관리해야 하는 요소이다.

기본 통신이 구축되면 ROS 2 스택은 Isaac Sim을 가상 로봇 플랫폼(virtual robot platform)으로 취급할 수 있다. 위치추정 알고리즘은 시뮬레이션된 오도메트리, IMU, GNSS 또는 LiDAR 관측값을 사용할 수 있고, 인식 노드는 렌더링된 카메라 영상이나 포인트 클라우드 데이터를 처리할 수 있다. 플래너(planner)는 시뮬레이션 환경에서 경로를 생성하고, 컨트롤러는 다시 운동 명령을 전달할 수 있다. 그 결과 실제 하드웨어가 준비되기 이전에도 자율주행 소프트웨어 스택의 상당 부분을 실행할 수 있는 소프트웨어 인 더 루프 환경(software-in-the-loop environment)이 구축된다.

이러한 접근 방식은 동일한 로봇 소프트웨어를 시뮬레이션과 실제 하드웨어 모두에서 사용해야 할 때 특히 중요하다. 하드웨어 추상화(hardware abstraction)와 안정적인 인터페이스는 애플리케이션 로직 내부에 포함되는 시뮬레이션 전용 가정을 줄여준다. 내비게이션 또는 인식 구성 요소는 입력 데이터가 Isaac Sim에서 생성되었는지 실제 센서에서 생성되었는지에 의존하기보다 정의된 ROS 2 인터페이스에 의존하는 것이 이상적이다. 이러한 원칙은 구성 요소화(componentization), 하드웨어 추상화, 혼합 시스템 통합(mixed system integration)을 요구하는 보다 광범위한 로봇 소프트웨어 아키텍처의 요구사항과 일치한다.

브리지 통합은 반복 가능한 자동 검증(repeatable automated validation)도 가능하게 한다. 스크립트로 작성된 Isaac Sim 시나리오는 정의된 위치에서 로봇을 초기화하고, ROS 2 노드를 시작하고, 센서 관측값을 생성하고, 명령을 실행한 후 결과를 기록할 수 있다. 이후 소프트웨어를 변경한 후에도 동일한 시나리오를 반복하여 동작의 회귀(regression)를 탐지할 수 있다. 헤드리스 시뮬레이션(headless simulation) 및 통제된 장면 생성(controlled scene generation)과 결합하면 ROS 2 통합은 단순한 대화형 데모 도구가 아니라 지속적인 테스트 인프라(continuous testing infrastructure)의 일부가 된다.

Physical AI 관점에서 브리지는 학습된 모델(learned models)을 현실적인 가상 상호작용(realistic virtual interaction)과 연결하는 경로를 제공한다. AI 인식 모델은 시뮬레이션 센서 스트림을 입력으로 사용할 수 있고, 학습된 정책(learned policies)이나 상위 수준 의사결정 시스템은 ROS 2 인터페이스를 통해 행동(action)을 생성할 수 있다. 합성 환경(synthetic environments)은 하드웨어 배포 이전에 서로 다른 객체, 조명 조건, 지형, 장애물, 운용 시나리오에 모델을 노출시킬 수 있다. 따라서 시뮬레이션은 단순한 오프라인 학습 데이터의 공급원을 넘어 AI를 둘러싼 실행 가능한 환경(executable environment)이 된다.

궁극적인 목표는 시뮬레이션에서 현실로 전환되는 과정에서도 유지되는 인터페이스 경계(interface boundary)를 구축하는 것이다. 개발 단계에서는 Isaac Sim이 가상 물리 환경, 센서, 로봇 상태를 제공하고, 이후 실제 로봇이 이에 대응하는 현실 세계의 신호를 제공한다. ROS 2는 계속해서 인식, 위치추정, 경로계획, 제어, 자율성(autonomy)을 연결하는 통신 프레임워크(communication framework)로 유지된다. 토픽, 프레임, 타임스탬프, 메시지 정의, 명령 인터페이스를 일관되게 유지하면 ROS 2 브리지는 소프트웨어 인 더 루프 테스트(software-in-the-loop testing), Sim2Real 검증, 그리고 점진적으로 완성되는 로봇 시스템 통합을 위한 실질적인 기반이 된다.

## 06.05. RTX Sensor Rendering LiDAR Camera in Isaac [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

RTX 기반 센서 렌더링(RTX-based sensor rendering)은 Isaac Sim에서 시뮬레이션된 환경으로부터 카메라 및 LiDAR 관측값(camera and LiDAR observations)을 생성하기 위한 물리 지향적 접근 방식(physically oriented approach)을 제공한다. 센서를 단순한 시각 효과(visual effects)로 취급하는 대신, 시뮬레이터는 가상 센서가 기하 형상(geometry), 재질(materials), 조명(lighting), 공간적 관계(spatial relationships)를 어떻게 관측하는지를 모델링한다. 이를 통해 센서 렌더링은 시뮬레이션된 물리 세계와 실제 로봇에서 사용될 인식 시스템(perception systems)을 연결하는 중요한 요소가 된다.

카메라 시뮬레이션(camera simulation)은 실제 비전 시스템(vision system)의 입력과 유사한 RGB 관측값(RGB observations)을 생성할 수 있다. 가상 카메라는 로봇 장면(robot scene) 안에서 정의된 위치, 방향, 시야각(field of view), 해상도(resolution), 갱신 주기(update frequency)를 가진다. 카메라가 장면 계층구조(scene hierarchy)를 통해 시뮬레이션된 로봇에 연결되어 있기 때문에 로봇이 움직일 때 카메라의 시점(viewpoint)도 자연스럽게 변화한다. 이를 통해 인식 알고리즘은 고정된 정적 이미지가 아니라 현실적인 로봇 움직임으로부터 생성된 영상 시퀀스(image sequences)를 처리할 수 있다.

RTX 렌더링(RTX rendering)은 환경의 시각적 외형(visual appearance)이 인식 성능(perception performance)에 영향을 미치는 경우 특히 유용하다. 조명, 그림자, 반사(reflections), 표면 재질, 객체 기하 형상 및 환경 조건은 생성되는 카메라 이미지에 영향을 줄 수 있다. 따라서 창고, 도로, 공장 또는 야외 지형을 기반 물리 장면(underlying physical scene)은 동일하게 유지하면서 서로 다른 시각적 조건으로 렌더링할 수 있다. 이러한 변화는 통제된 시뮬레이션 시나리오(controlled simulation scenarios)에서 인식 시스템의 강건성을 평가하기 위한 기반을 제공한다.

깊이 센싱(depth sensing)은 카메라 시뮬레이션을 외형에서 기하학적 인식(geometric perception) 영역으로 확장한다. 시뮬레이션된 깊이 카메라는 영상의 각 픽셀에 대응하는 거리 정보를 제공할 수 있으며, 이를 통해 알고리즘이 객체 표면과 공간 구조를 추정할 수 있다. RGB와 깊이 관측값(depth observations)은 동일한 가상 카메라 구성에서 생성될 수 있으므로 공간적 관계를 일관되게 유지할 수 있다. 이는 장애물 검출(obstacle detection), 3D 재구성(3D reconstruction), 매니퓰레이션(manipulation), 내비게이션(navigation), 센서 융합(sensor fusion)과 같은 응용에 유용하다.

LiDAR 시뮬레이션(LiDAR simulation)은 카메라 렌더링과는 다른 센싱 원리를 따른다. 시뮬레이션된 LiDAR는 투사된 빛을 기반으로 이미지를 생성하는 대신, 가상 환경의 객체와 상호작용하는 광선(ray) 또는 빔(beam)에 따라 거리 측정값(range measurements)을 생성한다. 생성된 포인트 클라우드(point cloud)는 센서 위치에서 관측한 공간 구조를 나타낸다. 스캔 패턴(scanning pattern), 각도 해상도(angular resolution), 측정 거리(range), 갱신 주기(update rate), 장착 변환(mounting transform) 등의 매개변수가 생성되는 측정값의 특성을 결정한다.

이동 로봇(mobile robot)에서는 LiDAR 장착 위치를 정확하게 표현하는 것이 중요하다. 센서 출력은 로봇 본체에 대한 센서의 관계에 직접적으로 영향을 받기 때문이다. 예를 들어 섀시 위에 장착된 LiDAR와 지면 가까이에 장착된 LiDAR는 서로 다른 영역을 관측한다. 따라서 로봇이 이동하고 회전하며 지형을 오르거나 장애물과 상호작용하는 동안에도 시뮬레이션은 센서 변환(sensor transform)을 정확하게 유지해야 한다. 잘못된 장착 매개변수(mounting parameters)는 의도한 실제 센서 구성과 상당히 다른 인식 동작을 만들어낼 수 있다.

RTX로 생성된 관측값을 AI 또는 인식 개발에 사용할 경우 센서 노이즈와 불완전성(sensor noise and imperfections)도 고려해야 한다. 완벽하게 깨끗한 시뮬레이션 이미지나 포인트 클라우드는 실제 센서의 특성을 충분히 표현하지 못할 수 있다. 노이즈, 측정 누락(missing measurements), 제한된 해상도, 움직임에 따른 효과(motion effects), 시간 차이(timing differences), 환경적 영향은 시뮬레이션과 현실 사이의 차이(simulation-to-reality gap)에 영향을 줄 수 있다. 따라서 보다 광범위한 센서 시뮬레이션 아키텍처에서는 시각적 사실성만으로 물리적 현실성이 보장된다고 가정하지 않고, 충실도(fidelity)와 노이즈 모델링(noise modeling)을 중요한 요소로 취급한다.

여러 센서를 결합하면 더욱 풍부한 가상 인식 시스템(virtual perception system)을 구축할 수 있다. RGB 카메라는 외형 정보를 제공하고, 깊이 센서는 기하학적 정보를 제공하며, LiDAR는 비교적 직접적인 거리 측정 정보를 제공할 수 있다. 여기에 IMU 또는 다른 시뮬레이션 측정값을 추가하여 움직임과 관련된 정보를 제공할 수도 있다. 이러한 센서들이 공통된 로봇 좌표 구조(robot coordinate structure)와 의미 있는 타임스탬프를 공유하면, 실제 다중 센서 로봇에서 수행하는 것과 유사한 방식으로 인식 및 위치추정 소프트웨어가 여러 센서의 출력을 결합할 수 있다.

여러 시뮬레이션 센서 스트림(sensor streams)을 함께 사용할 때는 타임스탬프 관리(timestamp management)가 특히 중요하다. 카메라, LiDAR 및 기타 센서는 서로 다른 주파수로 동작할 수 있으며 정확히 동일한 시뮬레이션 스텝에서 관측값을 생성하지 않을 수도 있다. 따라서 각 측정값은 의미 있는 시간 정보를 유지해야 하며, 이를 통해 하위 소프트웨어가 어떤 관측값들이 서로 연관되는지를 판단할 수 있어야 한다. 이는 움직임 추정(motion estimation), 센서 융합, 추적(tracking), 매핑(mapping) 및 시간적 일관성(temporal consistency)에 의존하는 기타 알고리즘에서 필수적이다.

RTX 센서 렌더링은 동일한 가상 장면에 통제된 변화를 반복적으로 적용하여 렌더링할 수 있기 때문에 합성 데이터 생성(synthetic data generation)도 지원한다. 객체 위치, 조명, 재질, 환경 조건 또는 로봇 자세를 변경하면서 카메라 이미지, 깊이 정보, 세그멘테이션 정보(segmentation information) 및 기타 센서 출력을 생성할 수 있다. 이렇게 생성된 데이터셋은 관측값과 시뮬레이션으로부터 직접 얻은 정보를 함께 제공할 수 있으며, 실제 환경에서라면 많은 비용이 필요한 수동 어노테이션(manual annotation)의 부담을 줄일 수 있다.

ROS 2와의 연결을 통해 시뮬레이션 센서 출력은 개발 과정의 다른 부분에서 사용되는 동일한 로봇 소프트웨어 아키텍처의 입력으로 사용할 수 있다. 카메라 영상, LiDAR 포인트 클라우드, 깊이 데이터 및 관련 센서 정보는 ROS 2 인터페이스를 통해 인식, 위치추정, 매핑, 경로계획(planning) 또는 AI 노드로 전달할 수 있다. 이를 통해 센서 렌더러는 독립적인 시각화 시스템이 아니라 실행 가능한 로봇 파이프라인(executable robotics pipeline)의 일부가 된다.

Physical AI 관점에서 RTX 센서 렌더링의 주요 가치는 시각 및 기하학적 시뮬레이션을 학습과 의사결정(learning and decision-making)에 연결한다는 데 있다. 인식 모델(perception model)은 카메라 또는 LiDAR 관측값을 입력으로 받아 주변 환경을 추정하고, 그 결과를 하위의 경로계획 또는 제어 구성 요소에 제공할 수 있다. 강화학습(reinforcement learning) 및 합성 데이터 워크플로우에서는 이러한 모델이나 정책(policy)을 다양한 센싱 조건에 노출시킬 수 있다. 이를 통해 인식 시스템의 동작을 실제 하드웨어로 전환하기 전에 평가할 수 있는 확장 가능한 환경을 구축할 수 있다.

최종적인 목표는 단순히 시각적으로 설득력 있는 센서 이미지를 생성하는 것이 아니라 제어 가능하고 반복 가능한 가상 센싱 시스템(controllable and repeatable virtual sensing system)을 구축하는 것이다. 로봇 자세, 센서 구성, 환경 기하 형상, 재질, 조명, 시간 정보, 노이즈 및 시뮬레이션 조건을 기록하고 재현할 수 있는 매개변수로 취급해야 한다. 이러한 접근 방식을 사용하면 RTX 카메라 및 LiDAR 렌더링은 Isaac Sim 기반 개발의 핵심 구성 요소가 되어 인식 검증(perception validation), 합성 데이터 생성, AI 학습, ROS 2 통합, 그리고 궁극적으로 Sim2Real 평가를 지원할 수 있다.

## 06.06. Isaac Gym Parallel RL Environment Setup [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

Isaac Gym은 한 번에 하나의 환경을 실행하는 대신 다수의 시뮬레이션 환경을 동시에 실행하는 GPU 중심 강화학습(GPU-oriented reinforcement learning) 접근 방식을 제공한다. 핵심 개념은 많은 수의 가상 로봇을 서로 다른 상태, 행동, 환경 조건에 노출시켜 높은 처리량(high throughput)으로 경험을 수집할 수 있도록 하는 것이다. 보다 넓은 Isaac 생태계에서 이러한 접근 방식은 물리 시뮬레이션(physics simulation), GPU 연산, 배치 환경(batched environments), 정책 학습(policy learning), 그리고 이후의 Sim2Real 전환을 연결한다.

병렬 강화학습 환경(parallel reinforcement-learning environment)은 공통된 로봇과 작업 정의(task definition)를 기반으로 이를 여러 개의 환경으로 인스턴스화(instantiation)하는 것에서 시작한다. 각 환경은 자체적인 로봇 상태, 객체 상태, 관측값(observations), 행동(actions), 보상(rewards), 에피소드 상태(episode status)를 유지하면서도 기본적인 계산 구조는 공유한다. 이를 통해 하나의 정책(policy)이 동시에 수많은 경험을 대상으로 학습할 수 있다. 예를 들어 보행 학습(locomotion task)에서는 동일한 로봇의 여러 복사본을 생성하고, 각각 서로 다른 자세, 속도, 지형 조건 또는 작업 매개변수에서 시작하도록 구성할 수 있다.

가장 중요한 아키텍처상의 장점은 시뮬레이션과 학습 연산을 배치(batch) 단위로 실행할 수 있다는 점이다. 각 환경의 상태를 CPU와 GPU 메모리 사이에서 개별적으로 이동시키는 대신 대규모 상태 집합을 텐서(tensor)로 표현하여 함께 처리할 수 있다. 물리 상태 업데이트, 관측값 구성, 보상 계산, 리셋(reset) 연산, 정책 추론(policy inference)을 벡터화된 연산(vectorized computation) 중심으로 구성할 수 있다. 이러한 접근 방식의 효율성은 시뮬레이션 파이프라인을 높은 수준으로 병렬화하고 불필요한 동기화(synchronization)나 데이터 이동(data movement)을 최소화하는 데 달려 있다.

환경 초기화(environment initialization)는 재현 가능한 학습을 위해 필요한 모든 매개변수를 설정해야 한다. 로봇 모델, 지형, 객체, 물리 속성, 센서, 제어 인터페이스, 에피소드 제한, 보상 정의, 초기 상태 분포(initial-state distributions) 등이 학습 루프가 시작되기 전에 구성되어야 한다. 수천 개의 환경을 생성할 경우 수동 설정은 현실적으로 불가능해진다. 따라서 프로그래밍 방식의 환경 정의(programmatic environment definition)가 필수적이며, 이를 통해 전체 환경의 구조를 일관되게 유지하면서 각 인스턴스 사이에는 통제된 차이를 부여할 수 있다.

랜덤화(randomization)는 동일한 환경만 사용하는 경우 제한적인 학습 다양성(training diversity)만 제공하기 때문에 특히 중요하다. 초기 로봇 자세, 속도, 객체 위치, 지형 특성, 마찰 매개변수, 질량, 액추에이터 특성, 센서 조건 및 기타 작업 변수를 여러 환경에서 다르게 설정할 수 있다. 이러한 변화는 정책이 학습 과정에서 보다 넓은 조건 분포(distribution of conditions)를 경험하도록 한다. 또한 단일 시뮬레이션 구성에 대한 과적합(overfitting)을 줄이고 실제 환경에 배포할 때 발생하는 변동성에 대비하는 데에도 중요하다.

관측 파이프라인(observation pipeline)은 시뮬레이션 상태를 정책이 사용할 수 있는 정보로 변환한다. 작업에 따라 관측값에는 로봇 위치, 방향, 선속도와 각속도, 관절 위치, 관절 속도, 접촉 정보, 센서 측정값, 목표 정보 또는 과거 상태가 포함될 수 있다. 관측 표현(observation representation)은 병렬 환경 전체에서 일관되게 유지되어야 하며, 정책은 동일한 의미 구조를 가진 관측값 배치(batch of observations)를 입력으로 받아야 한다. 학습 과정의 수치적 특성을 개선하기 위해 정규화(normalization)와 스케일링(scaling)을 적용할 수도 있다.

행동(actions) 역시 배치 형태로 표현된다. 정책은 여러 환경으로부터 수집된 관측값을 입력으로 받아 각 환경에 대응하는 행동 벡터(action vector)를 출력한다. 로봇과 작업에 따라 이러한 행동은 목표 관절 위치(target joint positions), 속도, 토크(torques), 휠 명령(wheel commands) 또는 기타 제어량(control quantities)을 나타낼 수 있다. 시뮬레이션은 이러한 행동을 적용하고 물리 상태를 진행시킨 다음 새로운 관측값을 생성하고 보상을 계산한다. 이러한 반복적인 상호작용이 모든 활성 환경에서 동시에 수행되는 기본 강화학습 루프를 형성한다.

대규모 환경을 사용할 때는 보상 계산(reward computation) 역시 벡터화되어야 한다. 각 환경은 작업 진행도, 안정성, 에너지 사용량, 추적 오차(tracking error), 접촉 동작 또는 기타 목적함수에 따라 보상을 계산한다. 종료 조건(termination conditions)은 해당 에피소드가 성공했는지, 실패했는지 또는 제한된 시간에 도달했는지를 결정한다. 종료된 환경은 다른 환경이 계속 실행되는 동안 리셋할 수 있으므로 전체 학습 과정은 높은 GPU 및 시뮬레이션 활용률을 유지할 수 있다. 모든 환경이 동시에 종료될 때까지 기다릴 필요가 없다는 점이 중요하다.

따라서 리셋 로직(reset logic)은 병렬 환경의 핵심 구성 요소이다. 리셋 과정에서는 로봇을 초기 상태로 복원하고, 작업 객체를 다시 생성하며, 새로운 지형 매개변수를 선택하고, 에피소드별 변수를 초기화할 수 있다. 효율적인 리셋은 전체 시뮬레이션을 불필요하게 다시 구성하지 않아야 한다. 대신 종료된 환경에 해당하는 상태만 복원하거나 수정하는 방식이 적절하다. 이를 통해 서로 다른 환경이 서로 다른 시점에 에피소드를 완료하더라도 지속적으로 경험 데이터를 수집할 수 있다.

보행(locomotion)의 경우 병렬 환경은 서로 다른 지형 프로파일, 경사, 표면 특성, 외란(disturbances) 또는 명령 속도를 표현할 수 있다. 따라서 사족보행 로봇(quadruped) 정책은 하나의 학습 반복 과정에서도 다양한 조건 조합을 경험할 수 있다. 매니퓰레이션(manipulation)에서는 객체 위치, 방향, 파지 구성(grasp configurations), 물리적 특성을 다양하게 설정할 수 있다. 이동 로봇에서는 서로 다른 장애물 배치, 목표 위치, 내비게이션 조건 또는 센서 구성을 표현할 수 있다. 관측값, 행동, 보상은 작업마다 달라지지만 동일한 아키텍처 원칙을 적용할 수 있다.

정책 추론(policy inference)은 배치 시뮬레이션 루프와 긴밀하게 통합되어야 한다. 시뮬레이터가 관측값을 생성하면 정책은 전체 관측 배치를 처리하고 각각에 대응하는 행동을 반환한다. 시뮬레이터는 모든 환경을 진행시키고, 이후 새로운 관측값과 보상을 생성한다. 이러한 연산을 가능한 범위에서 GPU 내부에 유지하면 통신 오버헤드(communication overhead)를 크게 줄이고 학습 처리량(training throughput)을 높일 수 있다. 결과적으로 이러한 아키텍처는 시뮬레이션과 학습을 완전히 독립적인 두 프로그램으로 처리하기보다 하나의 통합된 계산 파이프라인(computational pipeline)으로 취급한다.

대규모 병렬 학습(large-scale parallel training)은 계산 자원(computational resources)에 대한 세심한 관리도 필요로 한다. 환경의 수를 증가시키는 것이 항상 성능 향상으로 이어지는 것은 아니다. GPU 메모리, 물리 계산, 렌더링, 센서 처리, 정책 추론이 모두 동일한 자원을 사용하기 때문이다. 따라서 적절한 환경 수는 로봇 모델, 물리 복잡도, 관측 데이터 크기, 센서 구성 및 사용 가능한 GPU 용량에 따라 달라진다. 환경 스텝/초(environment steps per second), 정책 추론 시간, 메모리 사용량, 학습 진행도와 같은 의미 있는 처리량 지표를 이용하여 성능을 측정해야 한다.

학습 과정에서 모든 시뮬레이션 스텝마다 시각적 출력이 필요하지 않은 경우 렌더링을 핵심 강화학습 루프와 분리할 수 있다. 학습 실험은 주로 물리 상태와 간결한 관측값을 이용하여 실행하고, 렌더링은 디버깅(debugging), 평가(evaluation), 녹화(recording) 또는 선택된 환경에서만 활성화할 수 있다. 이러한 구분은 사실적인 렌더링(photorealistic rendering)과 고해상도 센서 시뮬레이션이 상당한 계산 자원을 소비할 수 있기 때문에 중요하다. 본 권에서 별도로 설명하는 센서 시뮬레이션 아키텍처는 시각적 관측값이 실제 학습 문제의 일부인 경우를 위한 기반을 제공한다.

이렇게 구성된 병렬 환경은 대규모 정책 학습(large-scale policy learning)을 위한 기반이 된다. 수천 개의 시뮬레이션 에피소드를 수집하는 동시에 축적된 경험을 이용하여 정책을 지속적으로 업데이트할 수 있다. 이는 보행, 매니퓰레이션, 내비게이션, 적응형 제어(adaptive control)와 같이 충분한 상호작용을 반복해야만 성공적인 행동이 나타나는 문제에 특히 유용하다. 이후의 Isaac Lab 및 대규모 병렬 시뮬레이션 주제에서는 이러한 개념을 보다 통합된 강화학습 프레임워크와 분산 GPU 실행(distributed GPU execution)으로 확장한다.

Physical AI 개발에서 중요한 결과는 단순히 학습 속도가 빨라지는 것이 아니라 확장 가능한 실험 아키텍처(scalable experimental architecture)가 구축된다는 점이다. 하나의 작업 정의를 이용하여 수많은 통제된 경험을 생성하고, 다양한 조건에서 정책의 동작을 평가하며, 체계적인 비교를 위한 체크포인트(checkpoints)와 평가 지표(metrics)를 확보할 수 있다. 이후 학습된 정책은 실제 하드웨어로 전환하기 전에 새로운 시뮬레이션 조건에서 평가할 수 있다. 따라서 Isaac Gym 방식의 병렬 강화학습은 GPU 가속 물리 시뮬레이션, AI 정책 학습, 강건성 평가(robustness evaluation), 그리고 보다 광범위한 Sim2Real 개발 프로세스를 연결하는 중요한 기반을 형성한다.

## 06.07. Replicator Synthetic Data Generation Pipeline [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

NVIDIA Omniverse Replicator는 통제된 가상 장면(controlled virtual scenes)으로부터 합성 데이터를 생성하기 위한 프로그래밍 가능한 프레임워크(programmable framework)를 제공한다. Isaac Sim 생태계에서 Replicator는 장면 에셋(scene assets), 로봇 모델, 카메라, 조명, 재질, 객체 배치, 의미론적 정보(semantic information), 랜덤화 매개변수(randomized parameters)를 자동화된 데이터 생성 과정으로 연결할 수 있다. 핵심 목적은 실제 환경에서 모든 사례를 수집하고 수작업으로 라벨링하는 데 필요한 비용과 시간을 줄이면서, 명확한 정답 데이터(ground truth)를 포함하는 대규모의 다양한 데이터셋을 생성하는 것이다. 본 장의 구조에서는 RTX 센서 렌더링과 병렬 강화학습 환경 다음에 Replicator를 배치하여 시뮬레이션과 AI 데이터셋 구축을 연결하는 실질적인 단계로 구성하고 있다.

Replicator 파이프라인은 목표 AI 문제와 관련된 객체 및 환경 요소를 포함하는 가상 장면에서 시작한다. 장면은 창고, 공장, 도로, 야외 환경, 매니퓰레이션 작업공간(manipulation workspace) 또는 기타 운용 영역(operational domain)을 표현할 수 있다. 고정된 가상 카메라가 아니라 로봇에 장착된 센서로부터 관측값을 생성하는 것이 목적이라면 로봇 모델과 센서 구성도 포함할 수 있다. 장면이 재사용 가능한 에셋과 USD 구성(USD composition)을 통해 표현되기 때문에 동일한 기본 환경을 활용하면서도 모든 데이터 샘플마다 완전히 별도의 장면을 만들지 않고 다양한 변형을 생성할 수 있다.

랜덤화(randomization)는 데이터셋의 다양성을 증가시키는 핵심 메커니즘이다. 객체의 위치, 방향, 크기, 텍스처, 재질, 조명 조건, 카메라 매개변수, 배경 및 환경 구성을 정의된 분포에 따라 변화시킬 수 있다. 생성된 데이터가 시뮬레이션된 상호작용에 의존하는 경우에는 물리적 매개변수도 변경할 수 있다. 여기서 목적은 통제되지 않은 무작위성이 아니라 AI 시스템이 실제 운용 환경에서 접할 수 있는 조건의 범위를 포함하도록 설계된 구조화된 변화(structured variation)를 만드는 것이다. 이러한 원칙은 본 권에서 정의한 보다 광범위한 합성 데이터 및 도메인 랜덤화(domain randomization) 전략과 일치한다.

효율적인 파이프라인은 장면 구성(scene configuration), 랜덤화, 렌더링(rendering), 어노테이션(annotation), 데이터 저장(data storage)을 분리한다. 장면 구성은 객체와 센서 설정을 정의하고, 랜덤화는 특정 시나리오 인스턴스(scenario instance)를 생성하며, 렌더링은 센서 관측값을 생성하고, 어노테이션은 해당 관측값과 연결된 정답 정보를 제공한다. 생성된 샘플은 어떤 장면 구성과 랜덤화 상태에서 생성되었는지를 식별할 수 있을 정도의 메타데이터(metadata)를 유지해야 한다. 이러한 분리는 파이프라인의 디버깅을 쉽게 하고 전체 데이터 생성 시스템을 다시 설계하지 않고도 개별 단계를 변경할 수 있도록 한다.

카메라 기반 합성 데이터(camera-based synthetic data)는 RGB 영상, 깊이 정보(depth information), 세그멘테이션 관련 출력(segmentation-related outputs)을 포함할 수 있다. 하나의 이미지에는 목표 인식 작업에 따라 객체 식별자(object identities), 클래스 라벨(class labels), 바운딩 정보(bounding information), 마스크(masks), 깊이 값 또는 기타 정답 표현(ground-truth representations)이 함께 제공될 수 있다. 시뮬레이터는 기본 장면 상태를 알고 있기 때문에 이러한 어노테이션을 프레임마다 수작업으로 생성할 필요 없이 자동으로 생성할 수 있다. 이것은 지도학습 기반 인식 개발(supervised perception development)에서 시뮬레이션 기반 데이터셋 생성이 갖는 핵심적인 장점 중 하나이다.

의미론적 정보(semantic information)는 렌더링된 픽셀을 시뮬레이션 객체의 논리적 정체성과 연결한다. 창고의 팔레트, 차량, 로봇, 장애물 또는 부품은 데이터 생성 과정에서 클래스 또는 의미론적 식별자(semantic identifier)를 가질 수 있다. 인스턴스 수준 정보(instance-level information)를 추가하면 동일한 클래스에 속하는 여러 객체도 서로 구별할 수 있다. 이러한 정보는 객체 검출(object detection), 의미론적 세그멘테이션(semantic segmentation), 인스턴스 세그멘테이션(instance segmentation), 자세 추정(pose estimation) 및 기타 컴퓨터 비전 작업에 유용하다. 최종 데이터셋의 품질은 의미론적 구조가 의도한 학습 문제를 정확하게 표현하는지에 따라 달라진다.

조명 랜덤화(lighting randomization)는 시각 AI에서 특히 중요하다. 동일한 객체라도 조명의 방향, 세기, 색온도(color temperature), 그림자 조건, 주변 반사(reflections)가 변화하면 영상에서 상당히 다르게 나타날 수 있다. Replicator 워크플로우에서는 하나의 고정된 조명 구성에 의존하지 않고 통제된 변화를 생성할 수 있다. 이를 통해 인식 모델이 보다 넓은 시각적 분포(visual distribution)를 경험하도록 하고 특정 조명 조건에 지나치게 의존하는지를 평가할 수 있다.

재질 및 텍스처 변화(material and texture variation)는 시각적 다양성을 제공하는 또 다른 방법이다. 기본 객체의 기하 형상을 유지하면서 표면 색상, 거칠기(roughness), 반사 특성(reflectance), 텍스처 패턴 및 재질 할당을 변경할 수 있다. 이는 AI 모델이 서로 다른 제조 표면, 환경, 마모 상태 또는 외형 영역(appearance domains)에서 객체를 인식해야 하는 경우 유용하다. 핵심은 시각적 속성을 체계적으로 변화시키면서 의미론적 일관성(semantic consistency)을 유지하여 생성된 라벨이 계속 정확하도록 하는 것이다.

카메라 구성(camera configuration) 역시 랜덤화 대상이 될 수 있다. 위치, 방향, 시야각(field of view), 초점 관련 특성(focal properties), 해상도 및 기타 센서 매개변수를 현실적인 범위 안에서 변화시킬 수 있다. 로봇 장착 카메라의 경우 이러한 변화는 제조 공차(manufacturing tolerances), 장착 차이 또는 센서 구성의 변경을 나타낼 수 있다. 다만 물리적으로 불가능한 구성을 생성하지 않도록 주의해야 한다. 비현실적인 샘플은 강건성을 향상시키기보다 학습 분포(training distribution)를 실제 운용 영역에서 멀어지게 만들 수 있기 때문이다.

동일한 개념은 LiDAR 및 기타 시뮬레이션 센서에도 확장할 수 있다. 서로 다른 시점, 스캔 구성, 환경 구조 및 객체 배치에서 LiDAR 관측값을 생성할 수 있다. 이후 포인트 클라우드 출력은 알려진 장면 기하 형상 및 의미론적 정보와 연결할 수 있다. 카메라, 깊이 센서, IMU 또는 기타 측정값과 결합하면 생성된 합성 데이터셋은 다중 모달 인식(multi-modal perception)과 센서 융합 연구를 지원할 수 있다. 앞서 설명한 RTX 센서 렌더링 아키텍처는 이러한 워크플로우를 위한 센싱 기반을 제공한다.

Replicator의 주요 장점 중 하나는 절차적 생성(procedural generation)이다. 수천 개의 장면을 수작업으로 구성하는 대신 스크립트를 통해 객체가 배치될 수 있는 위치, 생성할 객체의 수, 적용할 수 있는 재질, 조명 변화 또는 센서 매개변수 변화에 대한 규칙을 정의할 수 있다. 이후 생성기는 새로운 장면 상태를 반복적으로 만들고 관측값을 렌더링할 수 있다. 이를 통해 합성 데이터 생성은 수작업 콘텐츠 제작 작업에서 프로그래밍 가능한 데이터 파이프라인(programmable data pipeline)으로 전환되며, 데이터셋 요구사항에 따라 반복 실행하고 확장할 수 있다.

생성된 데이터셋은 메타데이터와 재현성 정보(reproducibility information)를 함께 관리해야 한다. 랜덤 시드(random seeds), 매개변수 분포, 장면 식별자, 에셋 버전, 센서 구성, 시뮬레이션 설정, 생성 타임스탬프 등을 생성된 샘플과 함께 보존할 수 있다. 이를 통해 문제가 있는 샘플이 어떤 조건에서 생성되었는지를 추적할 수 있다. 또한 인식 모델을 디버깅하거나 예상하지 못한 학습 결과를 조사할 때 특정 시나리오를 다시 재현할 수 있다.

합성 데이터 생성은 단순히 데이터셋의 크기로 평가해서는 안 된다. 좁거나 비현실적인 분포에서 생성된 매우 큰 데이터셋은 실제 운용 영역을 신중하게 표현하는 더 작은 데이터셋보다 유용성이 낮을 수 있다. 따라서 생성 전략을 설계할 때 객체 변화, 환경 조건, 센서 시점, 조명, 기하 형상 및 작업별 엣지 케이스(edge cases)의 범위를 고려해야 한다. 목표는 생성되는 이미지나 포인트 클라우드의 수를 단순히 최대화하는 것이 아니라 실제로 유용한 학습 분포(training distribution)를 구성하는 것이다.

실제 데이터(real data)는 합성 데이터의 대체재로만 취급하지 않고 함께 사용할 수 있다. 실제 데이터셋은 실제 센서 특성, 환경 외형 및 운용상의 변동성에 대한 근거를 제공하는 반면, 합성 데이터는 데이터 범위를 효율적으로 확장하고 자동으로 생성된 정답 정보를 제공할 수 있다. 따라서 하이브리드 전략(hybrid strategy)은 실제 샘플을 데이터 분포의 기준점(anchor)으로 사용하면서 실제 환경에서 수집하기 어렵거나 비용이 많이 드는 영역을 합성 샘플로 보완할 수 있다. 이후의 합성 데이터 장에서는 이러한 실제 데이터와 합성 데이터의 혼합 전략을 보다 명시적으로 다룬다.

파이프라인은 AI 학습 및 평가와 통합되어 생성된 데이터가 반복적인 개발 루프(iterative development loop)의 일부가 되도록 구성할 수 있다. 먼저 데이터셋을 생성하고, 인식 모델을 학습한 후, 검증 결과에서 약점이 발견되면 해당 약점을 집중적으로 평가할 수 있는 새로운 합성 시나리오를 생성할 수 있다. 이를 통해 시뮬레이션은 초기 학습 데이터를 생성하는 데만 사용되는 것이 아니라 어려운 조건을 체계적으로 탐색하는 데에도 사용된다. 이러한 접근 방식은 다양한 환경과 센서 조건에서 동작해야 하는 Physical AI 시스템에 특히 중요하다.

데이터셋 규모가 증가할수록 자동화(automation)가 더욱 중요해진다. 데이터 생성 작업은 배치(batch) 또는 헤드리스 모드(headless mode)로 실행할 수 있으며, 사용 가능한 GPU 자원에 분산하고 실험 설정별로 관리할 수 있다. 실패한 작업, 불완전한 출력, 데이터 생성 통계 등을 추적해야 대규모 데이터셋을 안정적으로 관리할 수 있다. 이후 동일한 인프라는 지속적 통합(continuous integration), 데이터셋 검증, 모델 학습, 회귀 테스트(regression testing)와 연결되어 보다 완전한 시뮬레이션-대-AI 개발 파이프라인(simulation-to-AI development pipeline)을 구성할 수 있다.

로봇공학에서 최종적인 목표는 합성 관측값이 의미 있는 물리적 및 운용 조건과 연결되어 있도록 하는 것이다. 생성된 이미지나 포인트 클라우드는 유효한 로봇 자세, 센서 구성, 환경 상태, 시뮬레이션 조건에 대응해야 한다. 물리적 상호작용이 포함되는 경우에는 장면이 로봇과 객체 사이의 물리적으로 타당한 관계도 표현해야 한다. 이러한 요구사항은 단순히 시각적으로 다양한 이미지 모음을 만드는 것과 실제 로봇의 운용 문제와 연결된 유용한 로봇 데이터셋을 만드는 것을 구분한다.

보다 넓은 Physical AI 워크플로우에서 Replicator는 시뮬레이션과 머신러닝(machine learning)을 연결하는 프로그래밍 가능한 브리지(programmable bridge) 역할을 한다. USD 기반 장면은 재사용 가능한 에셋을 제공하고, Isaac Sim은 물리 및 센서 시뮬레이션을 제공하며, RTX 렌더링은 사실적인 관측값을 생성하고, Replicator는 통제된 변화와 자동 어노테이션을 적용한다. 이렇게 생성된 데이터셋은 인식 및 AI 학습을 지원하고, 학습된 모델은 실제 환경으로 전환하기 전에 새로운 시뮬레이션 조건에서 평가할 수 있다. 따라서 합성 데이터 생성은 단순한 전처리 단계가 아니라 전체 시뮬레이션 생명주기(simulation lifecycle)에 통합된 핵심 개발 과정이 된다.

## 06.08. Omniverse Digital Twin Live Sync with Real Robot [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

Omniverse 디지털 트윈(Omniverse digital twin)은 가상 로봇 표현(virtual robot representation)을 실제 로봇의 상태와 연결하여 물리적 로봇의 운용 상태가 가상 환경에 지속적으로 반영되도록 할 수 있다. 디지털 트윈을 정적인 3D 시각화(static 3D visualization)로 취급하는 대신, 실제 텔레메트리(real telemetry), 로봇 상태(robot state), 센서 정보(sensor information), 설정 데이터(configuration data), 그리고 이에 대응하는 USD 기반 가상 장면(USD-based virtual scene) 사이의 관계를 지속적으로 유지한다. Omniverse와 Isaac Sim은 가상 표현을 제공하고, 외부 로봇 소프트웨어와 데이터 인터페이스는 이를 갱신하는 데 필요한 실시간 정보를 제공할 수 있다.

실시간 동기화(live synchronization)의 기반은 물리적 자산(physical assets)과 디지털 표현(digital representations) 사이에 명확한 대응 관계를 정의하는 것이다. 실제 AMR, 매니퓰레이터(manipulator), 사족보행 로봇(quadruped) 또는 기타 로봇은 구조, 좌표 프레임(coordinate frames), 센서, 액추에이터, 관련 속성이 일관되게 정의된 대응 가상 모델을 가져야 한다. 디지털 트윈은 어떤 가상 객체가 어떤 물리적 구성 요소를 나타내는지, 그리고 어떤 입력 데이터가 해당 상태를 제어하는지를 알아야 한다. 이러한 식별 및 매핑 계층(identity and mapping layer)이 없다면 실시간 동기화는 일관된 디지털 트윈이 아니라 서로 연결되지 않은 텔레메트리 스트림의 집합이 된다.

실시간 상태 미러링(real-time state mirroring)은 선택된 실제 로봇 상태를 가상 환경으로 전달한다. 위치, 방향, 속도, 관절 각도, 배터리 상태, 액추에이터 상태, 운용 모드(operating mode), 센서 상태 및 기타 텔레메트리를 대응하는 USD 또는 시뮬레이션 속성에 매핑할 수 있다. 그러면 가상 로봇은 실제 로봇의 상태에 따라 움직이고 변화할 수 있다. 목적은 반드시 모든 내부 하드웨어 변수를 복제하는 것이 아니라 시각화, 분석, 시뮬레이션, 진단(diagnostics), 운용 의사결정 지원(operational decision support)에 필요한 상태를 유지하는 것이다.

시간 동기화(time synchronization)는 디지털 트윈이 서로 관계없는 스냅샷의 연속이 아니라 변화하는 물리 시스템을 표현하기 때문에 필수적이다. 센서 측정값과 텔레메트리는 각각의 상태가 언제 존재했는지를 판단할 수 있도록 의미 있는 타임스탬프를 포함해야 한다. 로봇 시간, 센서 시간, 네트워크 시간, 시뮬레이션 시간 사이의 차이를 적절히 처리하지 않으면 가상의 움직임에 오류가 발생하거나 시스템 상태가 서로 일치하지 않는 것처럼 보일 수 있다. 따라서 견고한 아키텍처는 타임스탬프 출처(timestamp provenance), 동기화, 갱신 주기, 지연시간(latency)을 실시간 디지털 트윈 인터페이스의 명시적인 구성 요소로 취급한다.

네트워크 통신(network communication)은 실제 로봇과 디지털 트윈 사이의 데이터 경로(data path)를 구성한다. 시스템 아키텍처에 따라 텔레메트리는 Omniverse 환경에 도달하기 전에 ROS 2, 산업용 프로토콜(industrial protocols), 로봇 API, 메시지 브로커(message brokers), 엣지 컴퓨터(edge computers), 클라우드 서비스(cloud services) 등을 통과할 수 있다. 통신 계층에서는 고주파 상태 정보(high-rate state information)와 저주파 설정, 진단 또는 이벤트 데이터를 구분해야 한다. 이렇게 하면 모든 데이터 소스를 동일한 대역폭, 지연시간, 신뢰성, 갱신 특성을 요구하는 것처럼 처리하는 문제를 방지할 수 있다.

디지털 트윈은 수신되는 모든 측정값을 무조건 미러링해서는 안 된다. 상태 처리 계층(state-processing layer)은 수신 데이터를 가상 모델에 적용하기 전에 검증(validate), 필터링(filter), 변환(transform), 타임스탬프 처리, 데이터 조정(reconcile)할 수 있다. 로봇 로컬 좌표(robot-local), 맵(map), 월드(world), 시뮬레이션 프레임 사이에서 좌표 변환이 필요할 수도 있다. 잘못된 측정값, 오래된 패킷(stale packets), 일시적인 통신 손실 또는 센서 이상은 가상 장면에 그대로 전달하기보다 탐지해야 한다. 이를 통해 물리적 텔레메트리와 디지털 표현 사이에 통제된 경계(controlled boundary)를 구축할 수 있다.

USD 장면 계층구조(USD scene hierarchy)는 동기화된 로봇과 환경을 표현하기 위한 구조적 기반을 제공한다. 로봇 본체, 센서, 바퀴, 매니퓰레이터, 페이로드 및 주변 에셋은 식별 가능한 장면 요소(scene elements)로 구성할 수 있다. 실시간 상태 업데이트는 전체 장면을 다시 구성하지 않고 적절한 변환이나 속성만 수정할 수 있다. 이는 동일한 디지털 에셋이 시각화, 시뮬레이션, 과거 데이터 재생(historical replay), 예측 분석(predictive analysis), 가상 시나리오 분석(what-if experiments)을 지원해야 할 때 특히 유용하다.

센서 데이터(sensor data) 역시 단순한 로봇 상태 시각화 이상의 기능이 필요한 경우 디지털 트윈에 참여할 수 있다. 카메라, LiDAR, IMU, GNSS, 레이더 및 기타 센서 정보를 대응하는 시뮬레이션 센서 또는 가상 로봇 상태와 연결할 수 있다. 일부 애플리케이션에서는 디지털 트윈이 최신 측정값을 표시하고, 다른 애플리케이션에서는 해당 데이터를 처리하여 환경 객체를 갱신하거나 물리 세계와 가상 세계 사이의 일관성을 검증할 수 있다. 따라서 디지털 트윈의 주요 목적이 운용 모니터링(operational monitoring)인 경우에도 센서 동기화와 타임스탬프 관리는 여전히 중요하다.

실시간 동기화는 미러링되는 정보의 특성에 따라 서로 다른 갱신 주기(update rates)로 동작할 수 있다. 빠르게 변화하는 운동 상태(motion states)는 높은 빈도의 갱신이 필요할 수 있지만, 배터리 상태, 장비 구성(equipment configuration), 유지보수 정보는 훨씬 느리게 변화할 수 있다. 이러한 스트림을 분리하면 실제 요구사항에 따라 계산 및 네트워크 자원을 할당할 수 있다. 따라서 디지털 트윈은 모든 관련 데이터 소스가 동일한 주기로 동작하도록 강제하지 않고도 높은 빈도의 운용 상태를 유지할 수 있다.

유용한 디지털 트윈은 측정된 상태(measured state)와 시뮬레이션된 상태(simulated state)를 명확하게 구분한다. 물리적 텔레메트리는 실제 로봇이 현재 경험하고 있는 상태를 나타내는 반면, 시뮬레이션은 예측된 상태, 재구성된 상태 또는 가상의 조건(hypothetical conditions)을 표현할 수 있다. 두 정보를 명확한 출처 구분 없이 혼합하면 운용자는 표시된 값이 실제 로봇에서 측정된 것인지 시뮬레이션에서 생성된 것인지 알 수 없게 된다. 따라서 아키텍처는 중요한 데이터의 출처와 상태를 보존하여 측정값(measured), 추정값(estimated), 시뮬레이션값(simulated)을 정확하게 해석할 수 있도록 해야 한다.

실시간 동기화가 구축되면 디지털 트윈은 과거 데이터 재생(historical replay)과 운용 분석(operational analysis)을 지원할 수 있다. 기록된 로봇 상태를 가상 환경에서 재생하여 특정 사건을 조사하거나 특정 운용 조건을 재현할 수 있다. 동일한 가상 장면을 이용하여 로봇이 지형, 장애물, 다른 로봇 또는 설비와 어떻게 상호작용했는지를 분석할 수도 있다. 이를 통해 디지털 트윈은 단순한 실시간 시각화 도구에서 운용 데이터를 시뮬레이션과 연결하는 엔지니어링 분석 도구(engineering analysis tool)로 확장된다.

동기화된 디지털 트윈은 예측 시뮬레이션(predictive simulation)과 가상 시나리오 분석(what-if simulation)도 지원할 수 있다. 현재의 물리적 상태를 가상 실험의 초기 조건(initial condition)으로 사용하여 실제 로봇에 즉시 영향을 주지 않고 향후 행동, 환경 변화 또는 운용 전략을 평가할 수 있다. 예를 들어 플래너(planner)는 대체 경로를 평가하고, 유지보수 시나리오는 특정 구성 요소의 상태를 분석하며, 운용자는 변경된 작업 구성이 미치는 영향을 연구할 수 있다. 이러한 시뮬레이션은 의도적으로 폐루프 제어 아키텍처(closed-loop control architecture)를 구축한 경우가 아니라면 실시간 제어와 명확하게 분리되어야 한다.

플릿(fleet) 환경에서는 동일한 아키텍처를 하나의 로봇에서 여러 개의 물리적 자산으로 확장할 수 있다. 각 로봇은 개별 디지털 표현을 가질 수 있으며, 상위 수준의 Omniverse 환경은 공통된 운용 관점(operational view)을 제공할 수 있다. 플릿 수준 정보(fleet-level information)에는 로봇 위치, 임무 상태, 배터리 상태, 작업 할당(task assignments), 고장(faults), 환경과의 상호작용 등이 포함될 수 있다. 이는 이후 플릿 수준 디지털 트윈 아키텍처에서 개별 디지털 트윈을 집계하여 분석과 운용 조정을 수행하는 기반이 된다.

실시간 동기화는 보안과 데이터 무결성(data integrity) 요구사항도 발생시킨다. 디지털 트윈은 운용 중인 로봇으로부터 정보를 수신하고 시스템의 민감한 상태를 운용자 또는 다른 서비스에 노출할 수 있다. 따라서 인증(authentication), 권한 부여(authorization), 보안 통신(secure communication), 데이터 검증(data validation), 접근 제어(access control), 데이터 출처(provenance)가 운영 환경의 중요한 구성 요소가 된다. 또한 시스템은 안전하게 시각화할 수 있는 데이터와 실제 운용 제어나 보호 대상 정보를 노출할 수 있는 데이터를 구분해야 한다.

Physical AI 개발에서 실시간 디지털 트윈은 시뮬레이션과 실제 운용 세계를 연결할 수 있다. 실제 로봇은 실제 관측값과 상태를 제공하고, Omniverse와 Isaac Sim은 이러한 상태를 시각화, 분석, 재생하거나 추가 시뮬레이션의 초기 조건으로 사용할 수 있는 구조화된 가상 표현을 제공한다. 이를 통해 시뮬레이션을 고립된 개발 활동으로 취급하는 대신 실제 로봇(Real Robot), 실시간 상태(Live State), 디지털 트윈(Digital Twin), 시뮬레이션(Simulation), 분석(Analysis) 사이의 지속적인 관계를 구축할 수 있다.

최종 아키텍처는 단순한 데이터 피드(data feed)가 아니라 통제된 동기화 루프(controlled synchronization loop)로 이해할 수 있다. 실제 로봇의 상태는 센서와 텔레메트리를 통해 획득되고, 정의된 인터페이스를 통해 전달되며, 검증과 시간 정렬(time alignment)을 거친 후 USD 기반 디지털 표현에 매핑되어 Omniverse 안에서 시각화되거나 분석된다. 이후 시뮬레이션 결과는 측정된 실제 상태와 명확하게 분리된 상태에서 예측, 재생 또는 가상 시나리오 분석에 활용될 수 있다. 이러한 아키텍처는 기술(description) 중심의 디지털 트윈에서 동기화(synchronized), 예측(predictive), 그리고 궁극적으로 보다 자율적인 디지털 트윈 시스템으로 발전하기 위한 기반을 제공한다.

## 06.09. Isaac Lab Unified RL Training Framework [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Isaac Lab은 GPU 가속 시뮬레이션(GPU-accelerated simulation)을 기반으로 로봇 학습 정책(robot learning policies)을 개발하고 학습하기 위한 통합 프레임워크(unified framework)를 제공한다. 이는 Isaac Gym에서 소개된 병렬 환경(parallel environment) 개념을 확장하여 로봇 에셋(robot assets), 환경(environment), 관측값(observations), 행동(actions), 보상(rewards), 종료 조건(terminations), 학습 설정(training configurations)을 보다 구조화된 방식으로 정의할 수 있도록 한다. 보다 넓은 Isaac 생태계에서 Isaac Lab은 시뮬레이션, 강화학습(reinforcement learning), 작업 설계(task design), 센서 모델링(sensor modeling), 도메인 랜덤화(domain randomization), Sim2Real 개발을 하나의 공통 워크플로우(common workflow)로 연결한다.

이 프레임워크는 재사용 가능한 작업 및 환경 정의(reusable task and environment definitions)를 중심으로 설계된다. 하나의 학습 작업은 로봇, 장면, 물리 속성, 관측 공간(observation space), 행동 공간(action space), 보상 함수, 리셋 조건(reset conditions), 환경 매개변수를 정의하면서 동일한 구조를 여러 병렬 환경에 걸쳐 인스턴스화할 수 있다. 이러한 구조화는 강화학습 실험에서 지형, 객체, 로봇 구성 또는 보상 함수를 반복적으로 변경해야 하는 경우 특히 중요하다. 통합된 작업 표현(task representation)은 중복 구현을 줄이고 실험을 보다 쉽게 재현하고 확장할 수 있도록 한다.

Isaac Lab은 GPU 기반 병렬 시뮬레이션의 핵심적인 장점을 계승한다. 즉, 많은 환경을 동시에 진행시키면서 그 상태를 배치 기반 계산 구조(batched computational structures)로 표현할 수 있다. 여러 환경에서 생성된 관측값은 정책에 의해 함께 처리될 수 있으며, 그 결과 생성된 행동도 배치 형태로 반환될 수 있다. 따라서 시뮬레이션 스텝 진행, 관측값 생성, 보상 계산, 종료 처리, 환경 리셋이 하나의 고처리량 루프(high-throughput loop)에 참여할 수 있다. 이러한 구조는 매우 많은 상호작용이 필요한 학습 문제에 특히 적합하다.

환경 계층(environment layer)은 물리 시뮬레이션과 강화학습 알고리즘 사이를 연결하는 역할을 한다. 로봇이 어떻게 초기화되는지, 무엇을 관측할 수 있는지, 어떤 행동을 실행할 수 있는지, 작업이 어떻게 진행되는지, 언제 에피소드가 종료되는지를 정의한다. 예를 들어 보행 환경(locomotion environment)은 관절 상태, 본체 움직임, 접촉 정보, 지형 정보 및 속도 명령을 제공할 수 있다. 반면 매니퓰레이션 환경(manipulation environment)은 객체 자세, 엔드 이펙터 상태, 관절 상태 및 작업별 관측값을 제공할 수 있다. 동일한 프레임워크가 이러한 서로 다른 문제 구조를 모두 수용할 수 있다.

로봇 에셋(robot assets)은 통합 프레임워크의 또 다른 중요한 구성 요소이다. 로봇은 USD 에셋과 이에 연결된 물리, 시각, 액추에이터 및 센서 속성을 통해 표현할 수 있다. 이후 환경은 이러한 로봇을 공통 장면에 배치하고 지형이나 작업 객체와의 상호작용을 구성할 수 있다. 이러한 접근 방식은 앞서 설명한 USD 기반 로봇 장면 표현과 강화학습 작업 구성을 연결하여, 동일한 로봇 에셋을 전체 시뮬레이션 표현을 다시 구축하지 않고도 서로 다른 학습 실험에서 사용할 수 있도록 한다.

관측값과 행동 정의(observation and action definitions)는 시뮬레이션된 로봇과 학습 알고리즘 사이의 인터페이스를 제공한다. 관측값은 의도한 실제 배포 조건에서 정책이 사용할 수 있는 정보를 포함해야 하며, 행동은 목표 관절 위치, 관절 속도, 토크, 휠 명령 또는 기타 액추에이터 수준 명령(actuator-level commands)과 같이 실제로 제어할 수 있는 양과 대응해야 한다. 이러한 인터페이스를 명시적으로 유지하면 작업 로직(task logic)과 정책 구현(policy implementation)을 분리할 수 있으며, 동일한 시뮬레이션 작업에서 서로 다른 학습 알고리즘을 평가할 수 있다.

보상과 종료 로직(reward and termination logic)은 학습 목표와 각 에피소드의 경계를 결정한다. 보상 함수는 작업 진행도와 안정성, 추적 정확도, 에너지 사용량, 접촉 동작 또는 기타 측정 가능한 목표를 결합할 수 있다. 종료 조건은 작업의 성공적인 완료, 실패, 위험한 상태 또는 시간 제한을 식별할 수 있다. 이러한 계산은 많은 환경에서 동시에 실행되므로 작업 정의는 개별 시뮬레이션 에피소드를 각각 처리하는 방식보다는 효율적인 배치 계산(batched computation)을 지원하는 형태로 표현되어야 한다.

도메인 랜덤화(domain randomization)는 환경 구성에 직접 통합할 수 있다. 지형, 마찰, 질량, 액추에이터 매개변수, 객체 위치, 초기 상태, 센서 속성, 조명 및 기타 변수를 통제된 범위에서 샘플링할 수 있다. 목적은 정책이 하나의 고정된 시뮬레이션에만 적응하도록 하는 것이 아니라 다양한 조건의 분포(distribution of conditions)를 경험하도록 하는 것이다. 이는 가상 환경과 물리적 환경의 차이에 대한 민감도를 줄이기 위해 시뮬레이션 매개변수를 의도적으로 변화시키는 보다 광범위한 Sim2Real 방법론과 연결된다.

학습 정책이 시각적 또는 기하학적 정보에 의존하는 경우 센서 관측값(sensor observations)도 통합 학습 프레임워크의 일부가 될 수 있다. 카메라, 깊이 센서, LiDAR, IMU 및 기타 시뮬레이션 센서는 환경에 관측값을 제공하고, 환경은 이를 정책 입력으로 변환할 수 있다. 계산량이 많은 시각 학습에서는 렌더링과 센서 생성이 물리 계산 및 정책 추론과 GPU 자원을 경쟁할 수 있기 때문에 시뮬레이션 및 센서 파이프라인을 신중하게 설계해야 한다. 앞서 설명한 RTX 센서 렌더링 아키텍처는 이러한 시뮬레이션을 위한 기반을 제공한다.

Isaac Lab은 가능한 범위에서 환경 및 작업 정의와 강화학습 알고리즘을 분리한다. 동일한 작업을 완전한 시뮬레이션 환경을 다시 작성하지 않고도 서로 다른 정책 학습 방법이나 학습 설정과 연결할 수 있다. 이러한 분리는 체계적인 실험에 유용하다. 로봇, 관측값, 행동, 보상 및 물리 조건을 동일하게 유지하면서 서로 다른 알고리즘을 비교할 수 있기 때문이다. 또한 시뮬레이션 환경의 변화와 학습 방법의 변화로 인해 발생하는 결과를 구분하는 데 도움이 된다.

학습 설정(training configuration)은 정책 최적화(policy optimization), 롤아웃 수집(rollout collection), 배치 처리, 학습 스케줄, 체크포인트(checkpoints), 평가(evaluation), 실험 관리(experiment management)를 제어하는 매개변수를 포함한다. 이러한 매개변수는 애플리케이션 코드 곳곳에 분산시키기보다는 구조화된 설정으로 저장하는 것이 적절하다. 이렇게 하면 실험을 보다 쉽게 재현할 수 있고 많은 학습 실행(training runs)을 체계적으로 생성할 수 있다. 체크포인트는 중간 정책 상태를 보존하므로 학습을 재개하거나 평가하거나 개발 과정의 다른 단계로 전달할 수 있다.

평가(evaluation)는 학습과 별도의 단계로 취급해야 한다. 정책이 학습에 사용된 정확히 동일한 조건에서는 높은 보상을 얻더라도 지형, 객체, 외란(disturbances), 센서 조건 또는 초기 상태가 변경되면 다르게 동작할 수 있다. 따라서 Isaac Lab 방식의 워크플로우에서는 학습 분포(training distribution)를 넘어서는 조건을 의도적으로 테스트하는 평가 환경을 사용하는 것이 유용하다. 이를 통해 시뮬레이션 프레임워크는 정책을 최적화하는 것뿐만 아니라 실제 배포 전에 강건성(robustness)을 측정하는 데에도 활용될 수 있다.

통합 프레임워크는 서로 다른 로봇 종류가 하나의 공통 개발 인프라를 공유할 때 특히 유용하다. 사족보행 로봇의 보행 작업, 로봇 팔의 매니퓰레이션 작업, 이동 로봇의 내비게이션 작업, 그리고 보다 복잡한 전신 제어(whole-body control) 작업까지 공통된 환경, 관측값, 행동, 보상 및 학습 개념을 따를 수 있다. 로봇별 구현은 달라지지만 전체 실험 구조는 일관되게 유지할 수 있다. 이를 통해 보다 광범위한 Physical AI 학습 시스템을 위한 확장 가능한 기반을 구축할 수 있다.

GPU 활용률과 환경 수는 실제 작업 부하(workload)에 따라 관리해야 한다. 병렬 환경의 수를 증가시키면 경험 수집 처리량(experience throughput)이 증가할 수 있지만 GPU 메모리와 계산 자원도 함께 소비한다. 복잡한 관절형 로봇, 고차원 관측값, 카메라, LiDAR 및 복잡한 물리 계산은 동시에 실행할 수 있는 실제 환경 수를 감소시킬 수 있다. 따라서 성능 측정에서는 환경 수만을 주요 최적화 대상으로 취급하기보다 시뮬레이션 처리량, 정책 추론, 메모리 사용량, 렌더링 비용 및 학습 진행도를 함께 고려해야 한다.

헤드리스 실행(headless execution)은 대규모 Isaac Lab 학습에서 유용하다. 대부분의 강화학습 반복 과정에서는 모든 시뮬레이션 환경을 지속적으로 시각화할 필요가 없기 때문이다. 학습은 각 시뮬레이션 환경을 화면에 표시하지 않고 실행할 수 있으며, 선택된 에피소드 또는 평가 실행에서만 렌더링을 활성화할 수 있다. 이를 통해 불필요한 그래픽 오버헤드(graphical overhead)를 줄이고 사용 가능한 GPU 자원을 물리 계산, 센서 처리 및 정책 계산에 집중할 수 있다. 이후 시각화는 디버깅, 시연, 실패 분석 및 정성적 평가를 위해 선택적으로 사용할 수 있다.

이 프레임워크는 다중 GPU 및 대규모 시뮬레이션과도 자연스럽게 연결된다. 작업이 재사용 가능한 병렬 환경으로 표현되면 여러 환경 그룹을 사용 가능한 가속기 또는 시뮬레이션 프로세스에 분산할 수 있다. 이후의 대규모 시뮬레이션 장에서는 이러한 개념을 다중 GPU 분산 강화학습 학습, GPU 벡터화 물리 시뮬레이션, 클라우드 규모 시뮬레이션 팜, 자원 스케줄링(resource scheduling), 장애 허용 체크포인팅(fault-tolerant checkpointing)으로 확장한다. 따라서 Isaac Lab은 단일 GPU 병렬 학습과 보다 큰 규모의 분산 시뮬레이션 인프라 사이를 연결하는 중요한 중간 추상화 계층(intermediate abstraction)을 제공한다.

Sim2Real 개발에서 Isaac Lab은 통제된 시뮬레이션과 실제 로봇 배포 사이에서 정책 학습 계층(policy-training layer)의 역할을 수행할 수 있다. 정책은 먼저 대규모의 시뮬레이션 상호작용을 통해 학습하고, 이후 랜덤화된 조건 및 이전에 경험하지 않은 조건에서 평가하며, 마지막으로 보정된 로봇 및 센서 매개변수를 이용하여 전환을 준비할 수 있다. 이러한 과정의 효과는 학습 프레임워크 자체뿐만 아니라 기반 물리 모델, 센서 표현, 액추에이터 동작, 지연시간(delays), 도메인 랜덤화 전략의 품질에 좌우된다.

Isaac Lab의 보다 광범위한 의미는 단순히 또 하나의 강화학습 라이브러리가 아니라 통합된 실험 프레임워크(unified experimental framework)라는 데 있다. USD 에셋은 재사용 가능한 로봇 및 환경 표현을 제공하고, Isaac Sim은 물리 및 센서를 제공하며, 병렬 환경은 확장 가능한 상호작용을 제공하고, 작업 정의는 관측값과 목표를 지정하며, 학습 설정은 정책 최적화를 관리하고, 평가는 실제 배포를 향한 통제된 경로를 제공한다. 이러한 구조에서 시뮬레이션은 로봇 모델링, 강화학습, 강건성 테스트, 대규모 계산, 그리고 이후의 Sim2Real 전환 과정을 연결하는 반복 가능하고 재사용 가능한 AI 개발 플랫폼으로 기능한다.

## 06.10. Isaac Sim Large Scale Parallel Cluster Deployment

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

대규모 Isaac Sim 배포(large-scale Isaac Sim deployment)는 단일 시스템에서의 병렬 시뮬레이션 개념을 여러 시뮬레이션 작업을 동시에 실행할 수 있는 분산 컴퓨팅 아키텍처(distributed computing architecture)로 확장한다. 모든 환경을 하나의 GPU나 워크스테이션에 집중시키는 대신 시뮬레이션 작업을 여러 GPU, 컴퓨팅 노드(compute nodes), 클러스터 자원(cluster resources)에 분산할 수 있다. 이러한 접근 방식은 강화학습(reinforcement learning), 합성 데이터 생성(synthetic data generation), 회귀 테스트(regression testing), 대규모 시나리오 평가(large-scale scenario evaluation)가 단일 시스템이 제공할 수 있는 것보다 훨씬 높은 시뮬레이션 처리량(simulation throughput)을 요구할 때 특히 중요하다.

기본 아키텍처는 공통된 소프트웨어와 에셋 기반을 유지하면서 시뮬레이션 작업을 독립적으로 실행 가능한 작업(job)으로 분리한다. 각 작업에는 하나 이상의 Isaac Sim 환경, 로봇 모델, 센서, 물리 설정(physics configurations), 작업 정의(task definitions)가 포함될 수 있다. 스케줄러(scheduler)는 GPU 용량, 메모리 요구량, 실행 우선순위 및 기타 제약 조건에 따라 이러한 작업을 사용 가능한 컴퓨팅 자원에 할당한다. 이러한 분리 구조를 통해 로봇이나 시뮬레이션 모델 자체를 다시 설계하지 않고도 추가적인 컴퓨팅 노드를 연결할 수 있다.

클러스터 배포(cluster deployment)는 일반적으로 제어 계층(control layer), 스케줄링 계층(scheduling layer), 컴퓨팅 노드(compute nodes), 공유 또는 분산 스토리지(shared or distributed storage), 모니터링 서비스(monitoring services)로 구성된다. 제어 계층은 시뮬레이션 실험을 제출하고 관리하며, 스케줄러는 각각의 작업이 어디에서 실행될지를 결정한다. 컴퓨팅 노드는 Isaac Sim, 물리 계산, 렌더링, 센서 생성 및 AI 작업을 위한 CPU와 GPU 자원을 제공한다. 스토리지는 USD 에셋, 로봇 모델, 환경, 설정 파일, 체크포인트, 생성된 데이터셋에 대한 접근을 제공한다. 모니터링 시스템은 자원 상태와 작업 상태 정보를 통해 이러한 구성 요소들을 연결한다.

GPU 할당(GPU allocation)은 Isaac Sim 작업이 상당한 가속기 자원(accelerator resources)을 사용할 수 있기 때문에 핵심적인 고려사항이다. 서로 다른 실험은 병렬 환경의 수, 로봇의 복잡도, 센서 구성, 렌더링 요구사항, 강화학습 작업량에 따라 서로 다른 GPU 용량을 필요로 할 수 있다. 따라서 스케줄러는 단순히 사용 가능한 머신의 수를 계산하기보다는 GPU 메모리와 연산 능력을 명시적인 자원으로 취급해야 한다. 이를 통해 대규모 시뮬레이션 작업은 적절한 노드에 할당하고, 소규모 작업은 아키텍처가 허용하는 경우 사용 가능한 자원을 공유할 수 있다.

컨테이너화(containerization)는 분산 시뮬레이션을 위한 통제된 실행 환경(controlled execution environment)을 제공할 수 있다. Isaac Sim 애플리케이션, Python 의존성, 설정 파일 및 지원 소프트웨어를 재현 가능한 런타임 이미지(reproducible runtime image)로 패키징할 수 있다. 이후 클러스터는 여러 노드에서 동일한 소프트웨어 환경을 실행하면서 실험 설정만 변경할 수 있다. 이를 통해 운영체제 패키지, Python 환경, 드라이버 또는 애플리케이션 버전의 차이로 발생하는 불일치를 줄이고 대규모 실험을 보다 쉽게 재현할 수 있다.

USD 에셋 시스템(USD asset system)은 시뮬레이션 작업이 분산될 때 중요하다. 모든 컴퓨팅 노드는 호환 가능한 로봇 및 환경 에셋에 접근할 수 있어야 하기 때문이다. 대규모 환경에는 상세 메시(meshes), 재질(materials), 텍스처(textures), 센서 구성 및 물리적 속성이 포함될 수 있으며, 이러한 요소를 모든 작업마다 독립적으로 다시 구성해서는 안 된다. 공유 스토리지, 캐시된 에셋 또는 통제된 에셋 배포(asset distribution)는 반복적인 데이터 전송 오버헤드를 줄일 수 있다. 또한 자산 버전 관리(asset versioning)는 특정 실험이 정확히 어떤 로봇 및 환경 표현을 사용하여 실행되었는지 추적할 수 있도록 해야 한다.

병렬 강화학습 작업(parallel reinforcement-learning workloads)은 여러 워커(worker)로 분할할 수 있으며, 각 워커는 일정 수의 환경을 실행하고 관측값, 보상, 궤적(trajectories), 평가 지표(metrics) 또는 체크포인트를 반환한다. 학습 아키텍처는 선택한 학습 방법에 따라 이러한 결과를 집계하거나 분산된 정책 업데이트(distributed policy updates)를 수행할 수 있다. Isaac Lab은 단일 GPU 실행에서 다중 GPU 분산 학습으로 확장할 수 있는 작업 및 환경 추상화(task and environment abstraction)를 제공한다. Chapter 11의 보다 광범위한 구조에서는 이러한 발전을 GPU 벡터화 물리 시뮬레이션(GPU-vectorized physics) 및 대규모 시뮬레이션 인프라와 명시적으로 연결한다.

대규모 합성 데이터 생성(large-scale synthetic-data generation)도 동일한 클러스터 원리를 사용할 수 있다. 이미지, 깊이 맵(depth maps), 세그멘테이션 마스크(segmentation masks) 또는 포인트 클라우드를 하나의 워크스테이션에서 생성하는 대신 여러 GPU 노드에서 독립적인 생성 작업을 동시에 실행할 수 있다. 각 작업에는 서로 다른 랜덤 시드(random seed), 장면 구성(scene configuration), 카메라 자세, 조명 조건, 객체 배치 또는 환경 매개변수 범위를 할당할 수 있다. 이를 통해 통제되고 재현 가능한 생성 조건을 유지하면서 대규모 데이터셋을 분산 실행으로 생성할 수 있다.

시뮬레이션 수요가 사용 가능한 컴퓨팅 자원을 초과하면 작업 큐(job queue)가 필요하다. 작업은 필요한 GPU, 메모리, 예상 실행 시간, 우선순위, 데이터셋 저장 위치, 실험 설정 등의 정보를 포함하여 제출할 수 있다. 이후 스케줄러는 노드의 용량을 초과하지 않으면서 사용 가능한 자원에 대기 중인 작업을 배치할 수 있다. 큐 기반 실행(queue-based execution)을 사용하면 엔지니어가 모든 시뮬레이션 프로세스를 수동으로 시작하지 않아도 대규모 실험을 지속적으로 실행할 수 있다.

자원 스케줄링(resource scheduling)은 단순한 선입선출(first-come-first-served) 방식 이상의 것을 고려해야 한다. 대규모 강화학습 실험은 여러 GPU를 지속적으로 필요로 할 수 있는 반면, 소규모 회귀 테스트는 사용 가능한 자원의 일부만 필요로 할 수 있다. 따라서 스케줄링 정책(scheduling policy)은 작업 크기, 우선순위, 자원 가용성 및 예상 실행 특성을 고려할 수 있다. 적절한 스케줄링은 자원 활용률(utilization)을 높이는 동시에 불필요한 유휴 시간을 줄이고 대규모 작업이 모든 소규모 검증 작업을 차단하는 것을 방지한다.

헤드리스 실행(headless execution)은 클러스터 규모에서 특히 중요하다. 대부분의 강화학습 반복 과정과 많은 합성 데이터 생성 작업에서는 대화형 시각화(interactive visualization)가 필요하지 않기 때문이다. Isaac Sim은 모든 환경을 화면에 표시하지 않고 실행할 수 있으므로 GPU 자원을 물리 계산, 센서 생성, 필요한 경우의 렌더링, AI 계산에 집중할 수 있다. 시각화는 전체 클러스터에서 지속적으로 실행하기보다는 선택적인 평가 작업, 실패 조사, 정성적 검사(qualitative inspection), 디버깅을 위해 사용할 수 있다.

분산 시뮬레이션(distributed simulation)은 중앙 집중식 모니터링과 로깅(logging)도 필요로 한다. 각 작업은 실행 상태, 자원 사용량, 시뮬레이션 처리량, 오류, 출력 위치 및 관련 실험 지표를 보고해야 한다. 모니터링을 통해 과부하된 노드, 실패한 프로세스, 메모리 부족, 중단된 시뮬레이션 또는 비정상적인 처리량을 식별할 수 있다. 중앙 집중식 로그는 수백 또는 수천 개의 시뮬레이션 작업이 동시에 실행될 때 소프트웨어 오류와 인프라 문제를 구분하는 데에도 도움을 준다.

분산 작업의 수가 증가할수록 장애 허용(fault tolerance)이 더욱 중요해진다. 하나의 프로세스 실패가 전체 시뮬레이션 작업을 무효화해서는 안 된다. 따라서 작업은 중간 결과, 체크포인트 또는 완료된 데이터셋 파티션(dataset partitions)을 보존하여 실패한 작업을 이전의 모든 계산을 다시 수행하지 않고 재시작할 수 있도록 해야 한다. Chapter 11의 구조에서는 이러한 요구사항을 장애 허용 분산 시뮬레이션 체크포인팅(fault-tolerant distributed simulation checkpointing)으로 확장하며, 복구(recovery)를 사후적인 기능이 아니라 아키텍처의 기본 기능으로 취급한다.

데이터 이동(data movement)은 대규모 배포에서 중요한 병목이 될 수 있다. 시뮬레이션 노드는 대량의 카메라 이미지, LiDAR 포인트 클라우드, 깊이 맵, 궤적 및 진단 정보를 생성할 수 있다. 모든 출력 데이터를 즉시 중앙 스토리지 시스템으로 전송하면 GPU 계산이 아니라 네트워크 대역폭(network bandwidth)이 병목이 될 수 있다. 따라서 데이터 파이프라인은 필수적인 실시간 정보와 대용량 결과 데이터를 구분해야 하며, 네트워크 부하를 관리하기 위해 로컬 스테이징(local staging), 배치 전송(batch transfer), 압축(compression) 또는 분산 스토리지를 사용할 수 있다.

클라우드 인프라(cloud infrastructure)는 동일한 아키텍처를 온프레미스 클러스터(on-premise cluster) 외부로 확장할 수 있다. 로컬 용량이 부족하거나 일시적으로 높은 계산 수요가 발생할 때 시뮬레이션 작업을 클라우드 GPU 자원으로 전송할 수 있다. 클라우드 규모 시뮬레이션 팜(cloud-scale simulation farm)은 탄력적인 용량(elastic capacity)을 제공하여 작업 수요에 따라 활성 컴퓨팅 인스턴스의 수를 변경할 수 있다. 그러나 클라우드 실행이 적절한지를 판단할 때 스토리지 전송, GPU 가격, 인스턴스 시작 시간, 데이터 지역성(data locality), 작업 지속 시간이 중요한 요소가 된다.

비용 최적화(cost optimization)는 자원 활용률과 밀접하게 연결된다. 대규모 시뮬레이션은 추가적인 학습 또는 검증 가치를 거의 제공하지 않는 환경에 상당한 계산 시간을 사용할 수 있다. 적절한 환경 수, 적절한 GPU 선택, 헤드리스 실행, 작업 스케줄링, 체크포인트 재사용, 선택적 렌더링(selective rendering)을 통해 불필요한 계산을 줄일 수 있다. Chapter 11의 구조에서는 이러한 문제를 스팟 인스턴스(spot instances) 사용 및 시뮬레이션 비용 최적화와 연결하고 있으며, 이를 통해 경제적 효율성도 순수한 시뮬레이션 처리량과 함께 고려해야 함을 보여준다.

성숙한 배포 환경은 여러 개의 독립적인 워크스테이션이 아니라 하나의 시뮬레이션 클러스터(simulation cluster)로 운영될 수 있다. 실험 정의, USD 에셋, 로봇 모델, 환경 구성, 학습 매개변수 및 평가 프로토콜을 중앙에서 관리하면서 실행은 사용 가능한 GPU 자원에 분산할 수 있다. 결과는 공통 데이터 및 실험 관리 시스템으로 반환되어 대규모 시뮬레이션 실행 결과를 체계적으로 비교할 수 있다. 이를 통해 시뮬레이션은 대화형 엔지니어링 도구에서 확장 가능한 계산 인프라(scalable computational infrastructure)로 발전한다.

Physical AI 개발에서 대규모 Isaac Sim 배포는 전체 시뮬레이션 생명주기(simulation lifecycle)를 확장하기 위한 계산 기반을 제공한다. Isaac Sim은 물리 및 센서 시뮬레이션을 제공하고, Isaac Lab은 통합 강화학습 환경을 제공하며, Replicator는 합성 관측값을 생성하고, 분산 인프라는 이러한 작업을 훨씬 더 큰 규모로 실행할 수 있다. 결과적으로 하나의 공통 계산 환경에서 정책 학습(policy training), 인식 데이터 생성(perception-data generation), 강건성 평가(robustness evaluation), 회귀 테스트, Sim2Real 준비를 지원할 수 있다.

최종 아키텍처는 실험 정의에서 분산 실행으로 이어지는 계층 구조(hierarchy)로 볼 수 있다. 연구자는 로봇, 작업, 환경, 센서, 학습 또는 데이터 생성 설정, 평가 요구사항을 정의한다. 스케줄링 계층은 이러한 요구사항을 실행 가능한 작업으로 변환하고 사용 가능한 GPU 자원에 할당한다. 컴퓨팅 노드는 시뮬레이션을 실행하고, 모니터링, 스토리지, 체크포인트, 데이터 파이프라인은 결과를 수집하고 보존한다. 이러한 아키텍처는 단일 GPU 실험에서 다중 GPU 클러스터, 클라우드 규모 시뮬레이션 팜, 그리고 고도화된 Physical AI 시스템을 위한 대규모 병렬 시뮬레이션으로 발전하기 위한 기반을 제공한다.
