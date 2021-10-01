# SwiftGen을 통한 Asset 관리

### SwiftGen을 사용하면 storyboard/xib 혹은 코드에서 모두 사용 가능한 Assest을 Generate 가능!!


### How to use
* Requirements (cocoapods 설치 기준)
> pod 'SwiftGen', '~> 6.0'

* Step1. SwiftGen 설치
``` Ruby
pod install
```

* Step2. Configuration
> [터미널] 프로젝트 root 폴더로 이동
``` Ruby
cd $(YOUR_PROJECT_FOLDER)
```
> [터미널] SwiftGen Configuration / yml 파일 생성
``` Ruby
./Pods/SwiftGen/bin/swiftgen config init
```

* Step3. yml 파일 작성
> Step2까지 정상적으로 마친 경우 프로젝트 root 폴더에 'swiftgen.yml'파일이 생성된다.
> Project root folder 기준 Assets 파일 위치를 Input으로 설정해준다.
> Output에 생성될 폴더를 잡아준다 (원래 해당 폴더가 없으면 만들어져야하는데 그렇지 않아서 Generated 폴더를 비어있는 상태로 만들주었다.
> 그 후, swiftgen.yml 파일을 프로젝트로 드래그앤드롭 해준다 (Copy Option Uncheck!!) - 최상위 프로젝트 폴더 밑에 생성
``` Ruby
input_dir: StJohns/Resources
output_dir: StJohns/Resources/Generated/

## XCAssets
xcassets:
  inputs:
    - Assets.xcassets
    - Colors.xcassets
    - Images.xcassets
    
  outputs:
    templateName: swift5
    # 1
    params:
       # 2
       forceProvidesNamespaces: true
       # 3
       forceFileNameEnum: true
    output: XCAssets+Generated.swift

```

* Step4. Run Scripts 생성
> 1. Xcode의 좌측 리스트에서 최상위 Project 파일을 선택
> 2. TARGETS -> YOUR PROJECT -> BUILD PHASES 이동
> 3. + 버튼 -> 'NEW RUN SCRIPT PHASE' 클릭
> 4. Script의 이름을 'SwiftGen'으로 변경 (옵션)
> 5. Script 내용 작성
> 6. 이때 선택되어져 있어야 하는 옵션은 'Based on dependency analysis'와 'Show enviroment virables in build log'만 체크되어 있으면 된다.
``` Ruby
if [[ -f "${PODS_ROOT}/SwiftGen/bin/swiftgen" ]]; then
  "${PODS_ROOT}/SwiftGen/bin/swiftgen"
else
  echo "warning: SwiftGen is not installed. Run 'pod install --repo-update' to install it."
fi
```

* Step5. Generate
> 1. bulild 시 에러가 나지 않으면 성공적으로 생성 (저의 경우 프로젝트에 .이 들어가 있어서 고생했으나 .이 있는 경우는 무시하고 Input/Output 폴더를 잡아주면 됩니다.
> ex) 'my.test'의 프로젝트인 경우 --> 'input_dir: mytest/Resources'
> 2. 폴더에 output으로 설정한 Generated.swift 파일 확인
> 3. Generated.swift 파일을 Resources/Generated 밑으로 드래그앤드롭 후 Copy Option Check! (체크해야 매번 빌드되면서 동일한 파일이 옮겨진다.)

* Step6. Usage
``` Swift
let color = Asset.Colors.myColor.color
let image = Asset.Images.myImage.image
```
