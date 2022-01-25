# Semantic Versioning
````
MAJOR.MINOR.PATCH
````
- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards-compatible manner, and
- PATCH version when you make backwards-compatible bug fixes.
- Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.
- 1.0.0 버전은 공개 API를 정의한다. 이후의 버전 번호는 이때 배포한 공개 API에서 어떻게 바뀌는지에 따라 올린다.
- PATCH Z (x.y.Z | x > 0)는 반드시 그전 버전 API와 호환되는 버그 수정의 경우에만 올린다. 버그 수정은 잘못된 내부 기능을 고치는 것이라 정의한다.
- 공개 API에 기존과 호환되는 새로운 기능을 추가할 때는 반드시 MINOR Y(x.Y.z | x > 0)를 올린다.
- 공개 API의 일부를 앞으로 제거할 것(deprecate)으로 표시한 경우에도 반드시 올리도록 한다. 내부 비공개 코드에 새로운 기능이 대폭 추가되거나 개선사항이 있을 때도 올릴 수 있다. MINOR을 올릴 때 PATCH를 올릴 때만큼의 변화를 포함할 수도 있다. MINOR을 올라가면 PATCH를 0에서 시작한다.
- 공개 API에 기존과 호환되지 않는 변화가 있을 때는 MAJOR X(X.y.z | X > 0)를 올린다. MAJOR 번호를 올릴 때는 반드시 MINOR, PATCH를 0으로 초기화 한다.

- https://semver.org/lang/ko/