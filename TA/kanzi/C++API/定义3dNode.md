1 定义公共属性
例如：
//定义公共属性  
static PropertyType<Vector3> SimulationAreaSizeProperty;  
static PropertyType<Vector3> SimulationAreaCenterProperty;  
static PropertyType<float> BoundaryForceProperty;  
static PropertyType<float> BoundaryMarginProperty;  
static kanzi::PropertyType<float> TimeScaleProperty;  
static kanzi::PropertyType<bool> ShowBoundsProperty;  
static kanzi::PropertyType<bool> SimulationEnabledProperty;


2.定义宏数据——hpp
KZ_METACLASS_BEGIN(TestWorldNode,Node3D,"TestWorldNode")  
    KZ_METACLASS_PROPERTY_TYPE(SimulationAreaSizeProperty)  
    KZ_METACLASS_PROPERTY_TYPE(SimulationAreaCenterProperty)  
    KZ_METACLASS_PROPERTY_TYPE(BoundaryForceProperty)  
    KZ_METACLASS_PROPERTY_TYPE(BoundaryMarginProperty)  
    KZ_METACLASS_PROPERTY_TYPE(TimeScaleProperty)  
    KZ_METACLASS_PROPERTY_TYPE(ShowBoundsProperty)  
    KZ_METACLASS_PROPERTY_TYPE(SimulationEnabledProperty)  
KZ_METACLASS_END()


3.

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjE4NDM1NTM2XX0=
-->