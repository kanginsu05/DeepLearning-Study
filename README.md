# DeepLearning-Study

딥러닝 수업에서 진행한 Colab 실습 노트북과 개인 정리 노트입니다.
교재 **[Understanding Deep Learning](https://udlbook.github.io/udlbook/) (Simon J.D. Prince)** 의 공식 실습 노트북을 기반으로,
`TODO` 부분을 직접 구현하고 실행 결과와 개념 정리를 덧붙였습니다.

> 📝 **복습은 [`00_오픈북정리_전체요약.ipynb`](notebooks/00_오픈북정리_전체요약.ipynb) 부터** — 노트북별 한 줄 정리 표, 그림으로 보는 핵심 9가지, O/X 30문항(정답·해설)이 들어 있습니다.

## 학습 흐름

```
선형함수 → 선형회귀(손실) → 얕은 신경망(ReLU) → 깊은 신경망(행렬 표현)
   → 경사하강/SGD/Momentum → 역전파 → 정규화 → MLP 학습(MNIST-1D)
   → 합성곱/풀링 → CNN(MNIST) → Residual Network → Self-Attention
```

## 노트북 목록

| # | 노트북 | 주제 | 직접 구현한 내용 | 결과 / 상태 |
|---|---|---|---|---|
| 01 | [Background Mathematics](notebooks/01_Background_Mathematics.ipynb) | 선형함수, 행렬, exp/log | 1D·2D·3D 선형함수, 개별식 ↔ 행렬식 비교, exp/log 성질 확인 | ✅ 완료 |
| 02 | [Supervised Learning](notebooks/02_Supervised_Learning.ipynb) | 선형회귀, 최소제곱 손실 | 선형 모델, 제곱합 손실 함수, 파라미터 수동 피팅 | ✅ 손실값 정답(7.07)과 일치 |
| 03 | [Shallow Networks](notebooks/03_Shallow_Networks.ipynb) | 얕은 신경망, ReLU | ReLU, 은닉유닛 3개짜리 얕은 신경망 forward | ✅ 완료 |
| 04 | [Deep Networks](notebooks/04_Deep_Networks.ipynb) | 신경망의 행렬 표현, 네트워크 합성 | 1층·2층 네트워크를 `β, Ω` 행렬로 변환, 두 네트워크 합성 | ✅ 완료 (+ O/X 정리 주석) |
| 06 | [Gradient Descent](notebooks/06_Gradient_Descent.ipynb) | 경사하강, SGD, Momentum | Gabor 모델 손실 함수, 해석적 gradient 계산 | ⚠️ gradient 검증 완료 (수치미분과 일치). `gradient_descent_step`, momentum 업데이트는 미완성 |
| 07 | [Backpropagation](notebooks/07_Backpropagation.ipynb) | 역전파, chain rule | 다층 네트워크 forward / backward, 역전파 수식 정리(한글) | ✅ 유한차분 결과와 gradient 일치 |
| 08 | [L2 Regularization](notebooks/08_L2_Regularization.ipynb) | 과적합, L2 정규화 | 데이터 8개에 18차 다항식(파라미터 19개) 피팅, `λΣw²` 패널티 비교 | ⏸ 강의용 수정본. 실행 출력 없음 → 재실행 필요 |
| 09 | [MNIST-1D Performance](notebooks/09_MNIST_1D_Performance.ipynb) | MLP 학습, 초기화, 스케줄러 | `40→100→100→10` MLP, Kaiming 초기화, CrossEntropy | ✅ 50 epoch: train error 0.07% / **test error 36.5%** → 과적합 확인 |
| 10-1 | [2D Convolution](notebooks/10_1_2D_Convolution.ipynb) | 합성곱 직접 구현 | NumPy로 2D 합성곱 구현 (padding, stride, 다채널, batch) | ✅ 4가지 케이스 모두 PyTorch 결과와 일치 |
| 10-2 | [Downsampling & Upsampling](notebooks/10_2_Downsampling_Upsampling.ipynb) | 풀링, 업샘플링 | subsample, max/mean pooling, duplicate, max-unpooling, bilinear 보간 | ✅ 완료 |
| 10-3 | [CNN for MNIST](notebooks/10_3_CNN_for_MNIST.ipynb) | CNN 학습 | Conv(1→10)–Pool–ReLU–Conv(10→20)–Dropout–FC(320→50→10) | ✅ 3 epoch 후 **test 정확도 97.8%** |
| 11 | [Residual Networks](notebooks/11_Residual_Networks.ipynb) | Residual connection | `h ← h + f[h]` 형태의 residual forward | ✅ val error 32.4% (아래 복습 포인트 참고) |
| 12 | [Self-Attention](notebooks/12_Self_Attention.ipynb) | Query/Key/Value, scaled dot-product | — | ⏳ 미완성 (개념은 오픈북 정리에 있음) |

※ 5장(손실함수와 확률, negative log likelihood)은 별도 노트북 없이 오픈북 정리에 포함되어 있습니다.

## 배운 점

- **수식 → 코드 변환**: 얕은/깊은 신경망을 직접 행렬 연산(`np.matmul`)으로 구현하며 `β`, `Ω`가 각 층에서 하는 역할을 이해함
- **구현 검증 습관**: gradient는 유한차분(finite difference)으로, 합성곱은 PyTorch 결과와 비교해 내 구현이 맞는지 확인함
- **과적합 관찰**: MNIST-1D MLP에서 train error는 0%에 가깝지만 test error는 36.5% → 모델 크기/정규화/데이터의 중요성 체감
- **CNN의 효율**: 같은 MNIST 계열 문제에서 합성곱 구조가 적은 epoch로 높은 정확도(97.8%)를 냄

## 복습 포인트 (TODO)

- [ ] **06** `gradient_descent_step()` 구현 (`phi = phi - alpha * gradient`) 및 momentum 업데이트 완성
- [ ] **08** 노트북 재실행해서 λ 값별 곡선 비교 결과 남기기
- [ ] **11** `forward()`에서 `h3`가 `self.linear3` 대신 `self.linear2`를 다시 사용 중 → `linear3`로 고쳐서 결과 비교
- [ ] **10-2** `meanpool`의 내부 루프가 `x_in.shape[0]` 사용 → 정사각형이 아닌 입력 대비 `shape[1]`로 수정
- [ ] **12** Self-Attention (Q/K/V 계산, softmax, scaled dot-product) 구현

## 출처 및 라이선스

- 실습 노트북 원본: [udlbook/udlbook](https://github.com/udlbook/udlbook) — Copyright 2023 Simon Prince, MIT License ([LICENSE-udlbook](LICENSE-udlbook))
- 08번 노트북은 수업에서 재구성된 버전이며, 구현 코드·한글 주석·정리 노트는 개인 학습 기록입니다.
