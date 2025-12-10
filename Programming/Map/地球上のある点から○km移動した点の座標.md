---
type: programming
date:
links:
  - https://ikorihn.github.io/digitalgarden/note/Haversineの公式をGoで実装する
	- https://github.com/Turfjs/turf/blob/master/packages/turf-distance/index.ts
tags:
  - Programming
related:
in:
  - "[[MOC/Programming]]"
---

# Harversine公式の逆算をする

Haversine公式の逆算をして算出する

![image|500](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Illustration_of_great-circle_distance.svg/250px-Illustration_of_great-circle_distance.svg.png)

公式：
$$D = 2r \times \arcsin\left(\sqrt{\sin^2\frac{\phi_2 - \phi_1}{2} + \cos(\phi_1) \times \cos(\phi_2) \times \sin^2\frac{\lambda_2 - \lambda_1}{2}}\right)$$

- φ = 緯度、λ = 経度 を表す
- r は地球の赤道半径 正確には 6378137m で計算する

## TypeScript実装例

```ts
/**
 * 中心点から指定された距離（km）だけ東西南北に移動した点を計算する
 */
const createBoundingBox = (
  center: Coordinate,
  distanceKm: number,
): Feature<Polygon> => {
  const earthRadiusKm = 6371

  // 緯度方向の移動（北/南）
  const latDelta = (distanceKm / earthRadiusKm) * (180 / Math.PI)

  // 経度方向の移動（東/西）- 緯度によって補正
  const lngDelta =
    ((distanceKm / earthRadiusKm) * (180 / Math.PI)) /
    Math.cos((center.latitude * Math.PI) / 180)

  const north = center.latitude + latDelta
  const south = center.latitude - latDelta
  const east = center.longitude + lngDelta
  const west = center.longitude - lngDelta

  // GeoJSON形式: [longitude, latitude]の順
  return polygon([
    [
      [west, north], // 左上
      [east, north], // 右上
      [east, south], // 右下
      [west, south], // 左下
      [west, north], // 閉じる（左上に戻る）
    ],
  ])
}
```

## Turf.jsでの実装

Turf.jsの`distance`関数も内部的にHaversine公式を使用しており、同等の計算が可能。
https://github.com/Turfjs/turf/blob/master/packages/turf-distance/index.ts

```ts
import { distance } from '@turf/turf';

function turfDistance(
	point1: [number, number], // [lng, lat]
	point2: [number, number], // [lng, lat]
): number {
	return distance(point1, point2, { units: 'kilometers' });
}
```

---
