# ZIGBANG ZED Assignment

안녕하세요. 직방 ZED 팀 지원자 여러분. 지원해주셔서 감사합니다.

과제 설명 전 간단한 안내 말씀 드리겠습니다.

직방 ZED 팀은 유니티 엔진을 이용하여 직방에 있는 부동산 빅데이터들을

3D 정보로 시각화해주는 일을 하고 있습니다.

이와 관련하여 과제도 간단한 데이터를 이용해 3D 시각화로 진행하고 있습니다.

Github 의 fork 기능을 이용하여 개인 저장소로 복사해주신 다음 

코드를 작성 및 README.MD 파일 수정 후 개인 저장소의 URL을 채용 담당자에게 공유해주시면 됩니다.

최하단의 [문제 해결 방법] 섹션은 면접에 도움이 될 수 있도록

과제를 하면서 고민했던 부분, 해결방안 등에 대해 자유롭게 작성해주시면 됩니다.

## 과제 설명

### 문제1) 단지에 있는 동의 폴리곤 데이터를 이용하여 런타임에 메쉬를 구성하여 렌더링해주세요.

샘플 데이터는 아래의 경로에 위치해 있습니다.

Assets/Samples/json/dong.json

다음과 같이 구성되어 있습니다.

```
API Response 구성
{
    success: boolean, // API 성공 여부
    code: number, // API 결과 코드
    data: [] // 동 데이터 배열
}

동 데이터 구성
{
    roomtypes: [], // 타입 데이터의 배열
    meta: {
        bd_id: number, // 동 id
        동: string, // 동 이름
        지면높이: number, // 지면으로 부터의 높이
    } // 메타 정보
}

타입 데이터 구성
{
    coordinatesBase64s: string[], // 폴리곤 데이터의 배열
    meta: {
        룸타입id: number // 타입 id
    }
}

폴리곤 데이터의 구성 (*중요*)
- float[] 을 byte[] 로 변환후 Base64 로 인코딩된 데이터
- float[] 는 3개씩 나누어 Vector3 의 x,z,y 에 매핑됩니다.
  ex) [1.0, 2.0, 3.0, 4.0, 5.0, 6.0]
  -> [Vector3(1.0, 3.0, 2.0), Vector3(4.0, 6.0, 5.0)]
- C# 의 Buffer.BlockCopy, Convert.FromBase64String 등의 함수를 이용하시면 됩니다.
```

아래의 그림과 유사한 단지 구성이 되어야 합니다. 평형별 색상은 고려하지 않으셔도 됩니다.

![결과1](https://i.imgur.com/vcmmiid.png)



### 문제2) 문제1의 결과를 바탕으로 생성된 메쉬에 텍스쳐를 매핑합니다.

텍스쳐의 위치는 아래와 같습니다.

Assets/Samples/texture/buildingTester_d.png

텍스쳐는 아래와 같이 구성되어 있습니다.
![Texture](https://i.imgur.com/KOknmwQ.png)

건물의 면이 Vector3.forward 기준으로 200도를 향하고 있다면 거실이라고 가정합니다.

거실에 해당하는 면에는 1번 텍스쳐를 매핑합니다.

200도 계산시 +- 10% 정도의 오차를 허용합니다.

즉 정점의 normal 이 

180 <= Vector3.forward 기준 y 축 각도 <= 220 에 해당하면 1번 텍스쳐를 매핑합니다.

건물의 위/아랫면 즉 정점의 normal 이 Vector3.up / Vector3.down 인 경우에는 3번 텍스쳐를 매핑합니다.

그 외 면에는 2번 텍스쳐를 매핑합니다.

텍스쳐 파일을 자르지 않고 한장으로 그려야합니다.

아래의 그림과 비슷한 모양이 나와야합니다. 실 데이터와 차이가 있으므로 정확하게 일치하지는 않습니다.

![결과2](https://i.imgur.com/gbjA93E.png)

### 코드 설계시 우대되는 부분

유니티 라이프 사이클 함수 (Awake, Start, OnEnable, OnDisable, OnDestroy, Update) 이외의 직접 라이플 사이클을 구현하면 더 좋습니다.

# 문제 해결 방법 (자유롭게 작성)

