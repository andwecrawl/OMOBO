# TMDB
<br>

## 앱 소개
현재 인기 있는 영화나 프로그램을 소개해 주거나 좋아하는 영화를 검색시 관련 영화를 제공해 주며 현재 위치에 따라 주변 영화관의 위치를 알려주는 앱
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

### 1. completionHandler를 이용하여 데이터 전달 시기 맞추기

- 문제 상황
  - TMDB API에서는 장르를 ID로 전달해 줬기에, 해당 영화나 프로그램의 정보와 함께 장르를 보여주려면 API 통신을 한 번 더 거쳐야 했습니다. API 정보를 순서대로 가져온 뒤에 UI를 갱신해 주어야 했는데, 이 부분을 
- 문제 해결
  - 각 함수의 completionHandler를 이용하여 해결했습니다. completionHandler의 경우, 원하는 API가 성공적으로 호출되고 나서 실행되기 때문에 원하는 것처럼 API 정보를 순서대로 가져온 이후에 UI를 원하는대로 갱신해줄 수 있었습니다. 해당 로직은 아래와 같습니다.
  - API를 호출하는 Singleton 객체의 영화 장르와 TV 장르 배열이 비어 있을 경우 각각의 장르 배열을 먼저 가져온 뒤에 원하는 매체의 데이터를 호출하는 API를 호출하고, 비어 있지 않을 경우에는 바로 데이터를 호출합니다. 

```swift
    func callRequestCodable(page: Int = 1, segment: Trends, completionHandler: @escaping (TMDB, [[String]]) -> ()) {
        
        if TMDBManager.movieGenre.isEmpty && TMDBManager.tvGenre.isEmpty {
            
            self.callMovieRequest(url: URL.getGenreURL(media: .movie)) {
                self.callTvRequest(url: URL.getGenreURL(media: .tv)) {
                    self.callRequest(page: page, segment: segment) { data, genre in
                        completionHandler(data, genre)
                    }
                }
            }
            
        } else {
            
            self.callRequest(page: page, segment: segment) { data, genre in
                completionHandler(data, genre)
            }
        }
    }
```
<br>

### 2. prepareForReuse와 awakeFromNib을 활용하여 셀 초기화하기
- 문제 상황
  - 인물 정보를 호출할 경우, 셀을 재사용 시 간혹 값이 없는 경우가 있어 인물의 사진과 정보가 섞이거나 이상하게 뜨는 경우가 많았습니다.
- 문제 해결
  - 셀 재사용 시 호출되는 `prepareForReuse()`를 이용하여 이전 셀의 정보를 초기화하고 기본값을 넣어준 뒤에 정보를 넣도록 하여 해당 문제를 해결했습니다.

```swift
    override func prepareForReuse() {
        nameLabel.text = "정보를 찾을 수 없습니다."
        characterLabel.text = "정보를 찾을 수 없습니다."
        profileImageView.image =  UIImage(named: "noImage")
    }
```
