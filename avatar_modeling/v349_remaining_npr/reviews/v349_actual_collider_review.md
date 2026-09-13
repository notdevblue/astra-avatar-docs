> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 실제 콜라이더 재검토

읽기 전용 독립 검토. 2026-09-13. Original은 이전 v348 최종이며, v343 원본을 뜻하지 않는다. Final=0.3mm padding, Pad50=0.5mm padding. 모델/프로젝트 파일 수정·Unity 명령 없음.

## 판정

**0.3mm는 원래 Head/Neck 보호 capsule에 양수 여유를 확보하는 최소 우선 후보다.** 패딩 capsule 자체에는 여전히 작은 음수 간격이 있고, 반복 실행 변동이 있어 최종 채택은 반복/시각 검수 후 판단한다. padding을 ‘모든 콜라이더/헤어 메시 무관통’으로 표현하지 않는다.

실제 RUN은 error=null, Original/Final/Pad50 모두 완료했다. 각4800 active+30저장 warmup(실제90warmup), 총14400 active프레임을 독립 계산했다. 모든 source frame 연속0..4799, unity_frame 간격1, finite 통과, deltaTime0.0166666675, 원점/이동 입력차0이다. 초기 runner 미생성 실패 실행은 이번 raw파일/완료 판정에 포함하지 않았다.

| 후보 | Head: 원래 보호면 | Neck: 원래 보호면 | Head: 현재 capsule | Neck: 현재 capsule |
|---|---:|---:|---:|---:|
|Original(v348최종)|−0.025180mm|−0.219312mm|−0.025180mm|−0.219312mm|
|Final(+0.3mm)|**+0.229315mm**|**+0.135373mm**|−0.070685mm|−0.164626mm|
|Pad50(+0.5mm)|+0.423670mm|+0.330355mm|−0.076330mm|−0.169644mm|

Head 최악: Original Wind2.1833초(hair_01_02), Final Wind1.8초(같은본), Pad50 Wind9.8333초(같은본). Neck 세후보 최악 모두 Translation2.8167초 hair_tip_05, 무풍이다. 패딩0.3은 음수를 판정상 숨기기 위한 임계 변경이 아니라 실제 solver가 밀어낼 보호 범위를 넓힌 수정이며, 측정 기준을 원래면/패딩면으로 명시적으로 나눴다.

## 계산과 이전 근사의 차이

매프레임 JSON collider `position`은 TransformPoint(shape.position), 회전은 transform.rotation×shape.local_rotation, axis는 그 회전의Y축이다. 실제 기록된 lossyScale의 절대최대값으로 radius/height를 월드m로 바꿨다. capsule 중앙 선분 반길이=max(0,height/2−radius), 헤어 `_02`6개와`hair_tip_`6개의 구체반경4mm를 포함해 간격을 구했다. 현 prefab rootTransform=null/insideBounds=false/bonesAsSpheres=false가 확인된 범위다. 전체본선분/메시표면/SDK내부접촉로그를 대체하지 않는다.

원래 보호 capsule 비교는 같은 프레임의 현 콜라이더 실제 중심/축을 사용하되 Original의 반경/높이를 사용했다. 현재와 Original은 중심/축을 변경하지 않고 radius/height만 padding했으므로 이 비교가 적합하다. 일반적인 콜라이더 위치 변경 실험에서는 ‘원래면’도 별도 원본 행렬로 보존해야 한다.

lossyScale은 약100이고 축별 차이는 최대0.000030(100에대한상대3e−7)으로 float오차 수준이다. 기존 head/chest rest근사와 실제행렬 방식의 원래면 간격 차이는 최대 **0.000200mm 미만**이다. 따라서 기존 −0.025/−0.219mm가 Neck→Chest 재구성 때문에 생겼다는 가설은 이번 구간에서 기각한다. 실제 capsule에서도 같은 작은음수가확인된다. SDK 충돌보정후 길이제한/보간 등의 기여는 이전 IL조사상가능하지만, 내부solver단계별로그는없어기여비중을확정하지않는다.

## 움직임·복귀·반복 필요성

| 후보 | Hair Windtip 최대 | Tie Windtip 최대 | Hair 최대본각 | Tie 최대본각 | 바람종료후 .1mm내 정착 hair/tie |
|---|---:|---:|---:|---:|---:|
|Original|16.827260mm|15.865467mm|5.250351°|4.170130°|.417/.517초|
|Final|16.841626mm|16.003533mm|5.288981°|4.206392°|.417/.533초|
|Pad50|16.841626mm|16.003533mm|5.288981°|4.206392°|.417/.533초|

Hair본각은 실제변형본 hair_##_01/02만, windanchor를 제외했다. 최종warmup대비tip복귀0mm, 마지막1초 jitter0이다. 본angle25°/12°에근접하지않는다. peak같음이모든가닥움직임동일을뜻하지않는다.

**변경되지 않은 tie도 Original→Final peak약0.138067mm/최대각약0.036262° 차이**가 있어 이를 Head/Neck padding의 인과효과라고 해석하면 안 된다. Final/Pad50의요약수치가동일한것은두후보의모든표면이동일하다는뜻이아니다. 실행위상/초기화차이후보가남으며 `FinalRepeat`,`OriginalRepeat`를같은순차조건으로최소1회(가능하면3회)실행하는것이타당하다. 반복후각본의원래보호면최소가0을안정적으로넘는지확인한다. 현재+0.135mm Neck여유는기존실행변동0.063mm보다크지만, 그것을미래변동상한이라고보증할수는없다.

## 실제 표면80시각 비교 (Original→0.3mm)

기존v348표면검사방법을현재v349원시bins로독립재실행했다. Translation20/Turn20/Wind40의동일시각80개×2후보,헤어↔나머지BodyHair/Coat와tie영향삼각형↔나머지를검사했다. BVH후비공면정확교차선>0.05mm,같은원래위치를공유하는연결정점제외. 공면·표본사이연속·헤어자체전수는제외한다.

**새 pair0, 기존 pair제거3회, 기존교차선이0.05mm초과늘어난경우8회.** 새헤어–셔츠/넥타이–셔츠pair도0이다. 따라서‘새관통쌍은없음’과‘기존접촉을없앴음’을구분한다. 기존접촉쌍은시각별1121~1187개남아있으며이숫자는결함개수가아니다.

가장큰증가: **Wind1.5초 hair16183↔Coat3350**,교차선0.369359→0.677887mm(길이증가0.308528mm). component10/chain00의기존뒤목코트접촉지역과같다. 다음Translation3.0초동일pair2.108986→2.190992mm(+.082006). 나머지증가도뒤목코트경계15703/15672/15701/15700/15673/16175이다. 본래체인00에는Head/Neck패딩영향이작고시간위상차이도있어최종원인분리는반복자료를함께본다. 길이변화는관통깊이가아니다. 기존노출163회/40쌍을수정한것은아니므로별도뒤목시각/접촉작업은계속필요하다.

Pad50의표면80시각은이번에별도실행하지않았다. 이를채택하려면같은표면검사를추가한다. 최소후보Final이원래보호범위양수이고새pair0이므로0.5mm자동채택근거는없다.

## 파일/재현

- `/tmp/v349_actual_collider_review.py` → `/tmp/v349_actual_collider_review.json` (각원시JSON SHA256,14400frame간격/유한성/phase별최소/음수sample수/복귀)
- `/tmp/v349_padding_surface_review.py` → `/tmp/v349_padding_surface_review.json` (80개원래/최종교차쌍원시목록)
- 원시자료 `avatar_modeling/v349_remaining_npr/physics/{Original,Final,Pad50}_MOTION.json`, 같은폴더mesh_samples

```sh
.venv/mac_python/bin/python /tmp/v349_actual_collider_review.py
'[local home]/Library/Application Support/Steam/steamapps/common/Blender/Blender.app/Contents/MacOS/Blender' -b -t 2 --python /tmp/v349_padding_surface_review.py
```
