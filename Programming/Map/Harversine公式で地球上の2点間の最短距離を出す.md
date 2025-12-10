---
type: programming
date:
links:
  - https://ikorihn.github.io/digitalgarden/note/Haversineの公式をGoで実装する
tags:
  - Programming
related:
in:
  - "[[MOC/Programming]]"
---

# Harversine公式

Haversine公式は、地球上の2点間の最短距離（大円距離）を計算するための公式。

[Haversine formula - Wikipedia](https://en.wikipedia.org/wiki/Haversine_formula)

公式：
$$D = 2r \times \arcsin\left(\sqrt{\sin^2\frac{\phi_2 - \phi_1}{2} + \cos(\phi_1) \times \cos(\phi_2) \times \sin^2\frac{\lambda_2 - \lambda_1}{2}}\right)$$

- φ = 緯度、λ = 経度 を表す
- r は地球の赤道半径 正確には 6378137m で計算する

## TypeScript実装例

```typescript
// 地球の赤道半径(m)
const EARTH_RADIUS = 6378137;

function deg2Rad(deg: number): number {
	return deg * Math.PI / 180;
}

function haversineDistance(
	lat1: number,
	lon1: number,
	lat2: number,
	lon2: number
): number {
	const radLat1 = deg2Rad(lat1);
	const radLon1 = deg2Rad(lon1);
	const radLat2 = deg2Rad(lat2);
	const radLon2 = deg2Rad(lon2);
	
	const deltaLat = radLat2 - radLat1;
	const deltaLon = radLon2 - radLon1;
	
	const haversine = 
		Math.pow(Math.sin(deltaLat / 2), 2) +
		Math.cos(radLat1) * Math.cos(radLat2) * Math.pow(Math.sin(deltaLon / 2), 2);
	
	const distance = EARTH_RADIUS * 2 * Math.asin(Math.sqrt(haversine));
	
	return distance;
}
```

## テスト例

[２地点間の距離と方位角 - 高精度計算サイト](https://keisan.casio.jp/exec/system/1257670779) で計算した結果を使って確認する。

```typescript
describe('HaversineDistance', () => {
	const testCases = [
		{ pos: [32.460532, 148.277413, 43.082688, 135.780823], expected: 1611157.37 },
		{ pos: [43.589614, 127.127637, 34.173733, 146.384106], expected: 1963229.67 },
		{ pos: [32.950498, 132.119801, 33.664021, 133.077181], expected: 119339.55 },
		{ pos: [34.125722, 133.046361, 34.059234, 133.790232], expected: 68973.67 },
		{ pos: [34.125722, 133.046361, 34.129234, 133.090232], expected: 4061.54 },
		{ pos: [34.125722, 133.046361, 34.125634, 133.046232], expected: 15.4 },
	];
	
	testCases.forEach((testCase, index) => {
		it(`test case ${index}`, () => {
			const [lat1, lon1, lat2, lon2] = testCase.pos;
			const result = haversineDistance(lat1, lon1, lat2, lon2);
			expect(result).toBeCloseTo(testCase.expected, 1);
		});
	});
});
```

---
