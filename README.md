# TMDB
<br>

## 앱 소개
최근 유행하는 프로그램을 매채별로 알려 주고 주변 프로그램을 추천해 줍니다.
<br>
<br><br>

## 주요 기능
- 최근 유행하는 프로그램을 매체별로 알려줍니다.
  - TV 프로그램과 영화를 합친 전체 목록에서 인기 있는 프로그램을 추천해 줍니다.
  - 인기 있는 TV 프로그램과 영화, 인물을 순위별로 알 수 있습니다.
- 좋아하는 영화를 검색하면 비슷한 영화를 추천해 줍니다.
- 현재 위치를 핀으로 맵에서 표시해 주고, 근처 영화관의 위치 또한 핀으로 알려줍니다.
<br>
<br>


## 사용한 기술
- `UIKit`, `Storyboard`, `SnapKit`, `Compositional Layout`
- `TabMan`, `CoreLocation`, `MapKit`, `Alamofire`, `Kingfihser`
- `UserDefaults`, `MVC`, `Singleton`
<br>
<br>

## 기능 소개
- **Storyboard**와 **SnapKit**을 통한 **UI 구현**했습니다.
- TableView와 CollectionView의 **다양한 Cell**을 이용하여 **화면을 구성**했습니다.
- **Alamofire**를 이용하여 **다양한** TMDB **API**(Trends, Genere, Cast, Search, Recommendation) **통신 구현**했습니다.
- **CoreLocation**과 **MapKit**을 활용하여 **사용자의 권한 허용 여부에 따른 로직**을 구현했습니다.
  - 위치 권한을 허용하지 않았거나 이후 거부했을 경우 설정 화면으로 이동하도록 했습니다.
  - 위치 권한을 허용했을 경우 가까운 영화관의 위치를 Custom Annotation을 통해 보여주도록 구현했습니다.
<br>
<br>
## TroubleShooting
### 1. 트러블슈팅첫번째
