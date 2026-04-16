float  GlassSampleWet(float3 posWS)

{

float3 posOS = mul(_LiqWorldToLocal, float4(posWS, 1.0)).xyz;

//float3(1e-5, 1e-5, 1e-5)防止除零错误

float3 size = max(_LiqBoundsMaxOS.xyz - _LiqBoundsMinOS.xyz, float3(1e-5, 1e-5, 1e-5));

//将世界坐标转换为液体局部空间的UVW坐标，范围在0-1之间

float3 uvw = saturate((posOS - _LiqBoundsMinOS.xyz) / size);

return  SAMPLE_TEXTURE3D(_LiqWetTex, sampler_LiqWetTex, uvw).r;

}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MjI3NTI1Ml19
-->