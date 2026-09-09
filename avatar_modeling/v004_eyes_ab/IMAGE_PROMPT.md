> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# A: model-aligned eye texture

Built-in image_gen edit mode. Inputs, in order: input_geometry_guide.png (registered layout target), input_existing_color.png (palette only), avator_references/ref_char.png (style only).

Output: A_generated_eye_projection.png. This is a frontal projection source, not a UV atlas. It will be locally projected and baked into the unchanged model's original UV.

## Exact prompt

Use case: precise-object-edit.
Asset type: registered frontal diffuse/albedo projection for the eye region of an existing 3D anime avatar. This is a TEXTURE PAINTING task, not a new character or new 3D mesh.
Input 1 (gray clay head) is the EDIT TARGET and strict geometry/layout registration guide. Color this exact head without changing its framing, silhouette, facial proportions, camera, or the pixel positions of its modeled eyelids and iris discs.
Input 2 (colored head) is ONLY a skin/iris palette and identity reference. Its eye paint is misregistered: do NOT copy its high painted eye contour or duplicated lower contour.
Input 3 (full character) is ONLY supporting anime drawing style and muted brown eye color.
Primary request: create a clean, usable front-facing eye-area albedo matching the actual 3D eye openings in Input 1. Paint white/off-white sclera inside the actual modeled openings, muted brown irises exactly on the existing circular iris discs and slim black/brown eyeliner directly along the modeled upper/lower eyelid rims. Remove duplicated eyes, skin/eyelash paint on iris surfaces, white outline rings on skin, and dark hair smears. Both eyes must be coherently colored even though one will later be hidden by hair.
Invariants: preserve the gray guide's exact low-lidded eye shapes, spacing, eye corners, existing iris sizes and locations, eyebrow positions, nose and mouth positions, outer head silhouette, ears, bald head with flat upper cutoff, and square 1024x1024 framing. Eye paint must follow the LOWER real 3D eye openings, not the incorrectly higher printed eyes of Input 2. Do not beautify, enlarge eyes, lift eyebrows, smile, add hair, or change identity.
Output: one square image, same charcoal gray background and exactly registered head as Input 1. Pale warm skin matching Input 2; flat soft albedo colors with minimal shading, no hard directional lighting, no specular shine, no cast shadows, no text, no guides, no grid, no border. The intended result will be locally projected onto the UNCHANGED model and baked into its original UV map.
