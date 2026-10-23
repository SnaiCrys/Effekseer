# 1.80-patched

Base: upstream tag `1807`, commit `b87d1a2e3ff3731ba25cad34f8d8abeae382b21f`.

The DX11 `Renderer::Create` overloads accept an optional final
`bool premultipliedAlpha = false`. Set it to true when rendering Blend/Add
particles into a transparent intermediate target which will later be composited
with ONE / INV_SRC_ALPHA blending.

The option sets the existing `IsPremultipliedAlphaEnabled` implementation flag
before Initialize creates the DX11 blend states. RGB source blending is unchanged;
Blend accumulates coverage with alpha-over and Add preserves destination coverage.
The default retains upstream behavior. Source textures still use straight alpha.

Validated downstream with actual DX11 blend states: overlapping Blend particles
followed by Add, reversed depth, MSAA 1/2/4, differently sized output targets,
and a control detecting the upstream MAX-alpha intermediate-composition error.
