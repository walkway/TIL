# Defensive copy

- 클래스 클라이언트가 invariant 망가뜨릴 수 있으므로 방어적 프로그래밍을 해야한다.
- 생성자를 통해 초기화 할 때, 새로운 객체로 감싸서 복사 -> 외부와 내부에서 주소값을 공유하는 인스턴스 관계를 끊는다.
````
// 불변식 깨뜨리는 예시
Date start = new Date();
Date end = new Date();
Period period = new Period(start, end);
end.setYear(78); // period  내부 변경
````
-> 생성자로 전달되는 변경 가능 객체를 방어적으로 복사한다.
````
// 인자 유효성 확인 전 방어적 복사본을 만들고 -> 복사본에 대해 유효성 확인을 시행
public Period(Date start, Date end) {
	this.start = new Date(start.getTime());
	this.end = new Date(end.getTime());

	if (this.start.compareTo(this.end) > 0) {
		throw new IllegalArgumentException();
	}
}

public Date start() {
	return new Date(start.getTime());
}

public Date end() {
	return new Date(end.getTime());
}
````

- effective java #39