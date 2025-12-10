---
type: programming
date:
links:
  - https://github.com/Turfjs/turf/blob/master/packages/turf-destination/index.ts
tags:
  - Programming
related:
  - "[[Harversine公式で地球上の2点間の最短距離を出す]]"
in:
  - "[[MOC/Programming]]"
---

# Harversine公式の逆算をする

Haversine公式の逆算をして算出する

Harversineの公式については以下参考：
[[Harversine公式で地球上の2点間の最短距離を出す]]

## TypeScript実装例

```ts
// 地球の赤道半径(m)
const EARTH_RADIUS = 6378137;

function deg2Rad(deg: number): number {
	return deg * Math.PI / 180;
}

/**
 * 中心点から指定された距離（km）だけ東西南北に移動した点を計算する
 * 注意: これは近似的な計算。正確な逆算には方位角（bearing）を使った計算が必要
 */
function createBoundingBox(
	center: { lat: number, lng: number },
	distanceKm: number,
): { north: number; south: number; east: number; west: number } {
	const earthRadiusKm = EARTH_RADIUS / 1000;
	
	// 緯度方向の移動（北/南）
	const latDelta = (distanceKm / earthRadiusKm) * (180 / Math.PI);
	
	// 経度方向の移動（東/西）- 緯度によって補正
	const centerLatRad = deg2Rad(center.lat);
	const lngDelta = (distanceKm / earthRadiusKm) * (180 / Math.PI) / Math.cos(centerLatRad);
	
	const north = center.lat + latDelta;
	const south = center.lat - latDelta;
	const east = center.lng + lngDelta;
	const west = center.lng - lngDelta;
	
	return { north, south, east, west };
}
```

**逆算の確認:**
- 緯度方向: 距離dに対して角度差は `d / r` ラジアン = `(d / r) × (180 / π)` 度 ✓
- 経度方向: 緯度φでの経度方向の距離は `d / cos(φ)` で補正 ✓
- この計算は近似的に正しいが、厳密なHaversine公式の逆算ではない（方位角を使った計算が必要）

## Turf.jsでの実装

Turf.jsの`destination`関数は、起点（origin）から指定された距離（distance）と方位角（bearing）で移動した先の座標を計算する。

https://github.com/Turfjs/turf/blob/master/packages/turf-destination/index.ts

**主な処理:**
1. 入力座標と方位角をラジアンに変換
2. 距離をラジアン単位に変換（単位に応じて）
3. Haversine公式の逆算を使用して目的地の座標を計算:
   - 緯度: `asin(sin(φ₁) × cos(d) + cos(φ₁) × sin(d) × cos(θ))`
   - 経度: `λ₁ + atan2(sin(θ) × sin(d) × cos(φ₁), cos(d) - sin(φ₁) × sin(φ₂))`
   - ここで、φ₁, λ₁ = 起点の緯度・経度、d = 距離（ラジアン）、θ = 方位角（ラジアン）
4. 計算結果を度に戻して返す

**特徴:**
- 方位角（bearing）を使用するため、任意の方向への移動が可能
- 地球の曲率を考慮した正確な計算
- 距離の単位は `kilometers`（デフォルト）、`meters`、`miles`、`nauticalmiles`、`degrees`、`radians` が指定可能

```ts
import { point, destination } from '@turf/turf';

function turfDestination(
	center: { lat: number, lng: number },
	distanceKm: number,
): { north: number; south: number; east: number; west: number } {
  const centerPoint = point([center.lng, center.lat])

  // 北・東・南・西方向に distanceKm だけ移動した点
  const north = destination(centerPoint, distanceKm, 0, { units: 'kilometers' })
  const east  = destination(centerPoint, distanceKm, 90, { units: 'kilometers' })
  const south = destination(centerPoint, distanceKm, 180, { units: 'kilometers' })
  const west  = destination(centerPoint, distanceKm, -90, { units: 'kilometers' })

  const [, north] = north.geometry.coordinates
  const [, south] = south.geometry.coordinates
  const [east]    = east.geometry.coordinates
  const [west]    = west.geometry.coordinates

return { north, south, east, west }
}
```

---
