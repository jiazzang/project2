> # 2022 환경 데이터 분석·활용 공모전

## 1. 주제 정의
* 주제: 회귀모델을 활용한 시군구별 축사로 인한 환경오염지수 시각화
* 요약: 단계적 선택법(Stepwise selection)을 활용한 최적의 회귀모델을 구축하고, 회귀계수를 기반으로 시도, 시군별 가축 분뇨로 인해 발생하는 환경오염도 지수를 산출해 QGIS에서 지도 시각화  

## 2. 필요성 및 목적
### 문제 상황
* 가축 분뇨는 토양을 비옥하게 하고, 친환경적인 먹거리를 생산하는 순기능이 존재합니다. 그러나, 현재 가축 사육 규모가 커지면서 지역의 자연환경이 수용하기 어려울 정도로 많은 양의 분뇨가 발생하고 있습니다.
* 통제되지 않은 상황에서 농경지에 퇴비·액비를 무분별하게 살포하게 되면, 필요 이상의 양분(질소, 인 등)이 토양에 과잉으로 축적되고 이는 강우 시 지하수 및 주변 하천으로 유입되어 수질을 악화시킬 우려가 매우 큽니다.
* 특히, 악취를 동반하는 암모니아는 토양 및 수질 오염은 물론 자동차 등에서 나오는 가스상 물질과 결합해 2차 미세먼지를 만들어낼 수 있어, 전반적으로 환경 문제에 치명적인 영향을 줄 것으로 판단됩니다.

### 분석 목적
* 앞서 문제 상황 및 현황 파악을 통해, 지역이 소화할 수 있을 만큼의 분뇨 처리량과 지역별 축사 규모 기준에 관련된 정책을 마련해야 할 필요가 있다고 판단하였습니다.
* 이에 본 팀은 전국 시군구별 축사에 영향을 주는 요인들을 고려한 회귀분석 모형을 만들어, 도출된 회귀계수를 가중치로 활용해 해당 연도의 시군구별 환경오염지수를 산출한 후, 향후 지수 활용 방안도 제안하고자 합니다.

## 3. 활용 데이터 및 변수

<details>
<summary>활용 데이터 출처 및 데이터명</summary>
<div markdown="1">       


<details>
<summary>환경부</summary>
<div markdown="1">       

* 2020년도 기준 광역지자체별 가축분뇨 처리량.xlsx
* 2020년도 기준 광역지자체별 가축분뇨 처리농가수.xlsx
* 2020년도 기준 광역지자체별 가축분뇨 발생량.xlsx
* 2020년도 기준 광역지자체별 가축사육 농가수 및 두수.xlsx

</div>
</details>

<details>
<summary>물 환경 정보 시스템</summary>
<div markdown="1">       

* 해당 페이지에서 크롤링을 통해 전국 시군구에 위치해 있는 하천에서 검출된 질산성질소, 암모니아성질소, 총대장균수, 군원성대장균 총 4종의 오염물질 수치값을 가져옴 (2020년 기준)
* 해당 페이지에 존재하지 않는 시군구별 하천 오염물질 수치값은 시도별 하천 오염물질 수치값으로 채움

</div>
</details>

<details>
<summary>공공데이터 포털</summary>
<div markdown="1">       

* fulldata_02_04_01_P_가축사육업.csv (전국 가축업 현황 데이터)
* fulldata_09_30_01_P_가축분뇨수집운반업.csv(전국 가축분뇨수집운반업 현황 데이터)
* 한국환경공단_공공하수처리시설 현황_20201231.csv(전국 하수처리 시설 현황 데이터)

</div>
</details>
  
<details>
<summary>기상청</summary>
<div markdown="1">       

* 기상청 홈페이지에 접속하여 ‘기후통계분석 → 기상현상일수 → 폭염일수’ 페이지에서 ‘2020년도 전국 폭염일수.csv’ 파일을 가져옴

</div>
</details>

<details>
<summary>농림축산식품부, 국가가축방역통합시스템</summary>
<div markdown="1">       

* 농림축산식품부 홈페이지의 ‘가축질병 발생현황’ 페이지에서 아프리카 돼지열병, 조류인플루엔자, 구제역 데이터 크롤링을 진행함
* 국가가축방역통합시스템 홈페이지에서 질병 발생 농가 데이터 크롤링을 진행함

</div>
</details>  
  
<details>
<summary>국가정보포털-오픈마켓</summary>
<div markdown="1">       

* 국가정보포털-오픈마켓 홈페이지에서 전국 읍면동.shp 파일을 다운로드한 후 QGIS 상으로 가져와 전국 읍면동별 면적을 계산함
* 해당 페이지에 존재하지 않는 시군구별 면적 데이터는 시군구별 행정복지센터 홈페이지에서 직접 면적 값을 가져옴

</div>
</details>   

<details>
<summary>행정안전부</summary>
<div markdown="1">       

* 202012_202012_주민등록인구 및 세대현황_연간.xlsx (2020년도 전국 읍면동별 거주자 인구 등록 현황 데이터)

</div>
</details>  

<details>
<summary>KOSIS</summary>
<div markdown="1">       

* 시도, 시군별 1~12월 대기오염도 데이터
* 2020년도 읍면동 인구 데이터
* 2020년도 가축 종사자 수 데이터
* 2020년도 시도, 시군별 강수량 데이터
* 시군, 읍면동별 면적 데이터

</div>
</details>  
  
<details>
<summary>토양지하수정보시스템, 한국환경정책평가연구원</summary>
<div markdown="1">       

* 2020년도 시도, 시군, 읍면동을 기준으로 전국 토양 오염도 데이터를 가져옴

</div>
</details>   

<details>
<summary>AirKorea</summary>
<div markdown="1">       

* 2020년 시군구별 대기오염도 결측치를 채울 때 해당 사이트를 참고함

</div>
</details>   


</div>
</details>

<details>
<summary>활용 변수</summary>
<div markdown="1">       

<details>
<summary>독립변수</summary>
<div markdown="1">       

* 시군구별 가축 관련: 가축분뇨발생량(`가축분뇨발생량_합계`), 정화를 통한 가축분뇨 처리량(`분뇨처리_정화`), 퇴비를 통한 가축분뇨 처리량(`분뇨처리_퇴비`), 시군구별 가축 질병 발생 건수(`질병발생`), 시군구별 가축 더위지수(`시도별_가축더위지수`)
* 시군구별 가축 농가 관련: 시군구별 가축 사육 종사자 수(`가축사육종사자수`), 시군구별 농가 수(`농가수`)
* 시군구별 처리시설: 시군구별 하수처리시설 개수(`하수처리시설_개수`), 시군구별 분뇨처리업장 개수(`분뇨처리업장_개수`)
* 시군구별 가축 두수: 한우(`두수_한우`), 돼지(`두수_돼지`), 닭&오리(`두수_닭_오리`), 말(`두수_말`), 합계(`두수_소계`)
* 시군구별 특징 관련: 시군구별 면적(`읍면동_면적`), 시군구별 총 거주자 수(`읍면동_총거주자수`)
* 시군구별 기상 관련: 폭염 일수(`폭염일수_2020`), 시군구별 강수량(`강수량_2020`)

</div>
</details>

<details>
<summary>종속변수</summary>
<div markdown="1">       

* 시군구별 수질오염도(`시군별_수질오염도`): 시군구별로 측정된 특정 지하수 및 하천들의 총대장균군수, 질산성질소, 암모니아성질소의 농도의 합계를 구한 후, 평균값을 계산하여 사용함
* 시군구별 대기오염도(`시군별_대기오염도`): 시군구별로 측정된 미세먼지, 아황산가스, 이산화질소의 농도의 합계를 계산하여 사용함
* 시군구별 토양오염도(`시군별_토양오염도`): 시군구별로 측정된 특정 시설들의 토양 속 Cu, Zn, Ni 농도의 합계를 구한 후, 시설들의 평균값을 계산하여 사용함

</div>
</details>

<details>
<summary>파생변수</summary>
<div markdown="1">       

* 파생변수(독립변수): 시군구별 면적 대비 농가 수(`농가수_면적비`), 시군구별 면적 대비 분뇨발생량 합계(`분뇨발생량_합계_면적비`), 시군구별 면적 대비 총 거주자 수(`읍면동_총거주자수_면적비`), 시군구별 면적 대비 가축 두수 합계(`두수_소계_면적비`)
* 파생변수(종속변수): 시군구별 토양·대기·수질오염도 수치를 모두 합한 파생변수(`sum`)

</div>
</details>


</div>
</details>

## 4. 분석 과정
#### 최종 분석 데이터셋 생성
* 수집된 데이터를 병합하여 최종 분석 데이터셋

#### [데이터 전처리 및 분석 코드](https://github.com/jiazzang/Contest_2022_environment_data_analysis/blob/2fb5dc0410852cc0de04eb0912d7a85184fbb107/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EC%A0%84%EC%B2%98%EB%A6%AC_%EB%B0%8F_%EB%B6%84%EC%84%9D.ipynb)
* 결측치 대체
* 정규화 및 표준화
* 파생변수 생성
* 최적 회귀모형 선별
* 시군구별 환경오염지수 산출

## 5. 분석 보고서
#### [2022 환경 데이터 분석·활용 공모전 - 분석 보고서](https://elated-production-7bb.notion.site/2022-5861322b6e0a47b9a9bb6a471e656089)
