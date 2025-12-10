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

interface Point {
	lat: number;
	lng: number;
}

function haversineDistance(
	point1: { lat: number, lng: number },
	point2: { lat: number, lng: number },
): number {
	const point1Rad = {
		lat: deg2Rad(point1.lat),
		lng: deg2Rad(point1.lng),
	};
	const point2Rad = {
		lat: deg2Rad(point2.lat),
		lng: deg2Rad(point2.lng),
	};
	
	const deltaLat = point2Rad.lat - point1Rad.lat;
	const deltaLon = point2Rad.lng - point1Rad.lng;
	
	const haversine = 
		Math.pow(Math.sin(deltaLat / 2), 2) +
		Math.cos(point1Rad.lat) * Math.cos(point2Rad.lat) * Math.pow(Math.sin(deltaLon / 2), 2);
	
	const distance = EARTH_RADIUS * 2 * Math.asin(Math.sqrt(haversine));
	
	return distance;
}
```

---
