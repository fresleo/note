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


3.描述studio如何进行编辑当前元数据
static kanzi::PropertyTypeEditorInfoSharedPtr makeEditorInfo();

4.实现定义的propertytype：

例如：
PropertyType<Vector3> TestWorldNode::SimulationAreaSizeProperty(  
 kzMakeFixedString("TestWorldNode.SimulationAreaSize"),  
    Vector3(20.0f, 10.0f, 20.0f), 0, true,  
    KZ_DECLARE_EDITOR_METADATA(  
        metadata.displayName = "Simulation Area Size";  
        metadata.tooltip = "Width, height, and depth of the boid simulation bounding box.";  
        metadata.category = "Boids World";  
        metadata.host = "TestWorldNode:freq";  
        metadata.defaultValue = "20, 10, 20";  
        metadata.editor = "Vector3dFieldEditor.PropertyGridEditor";  
    ));
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzA1ODg0MzEsNjE4NDM1NTM2XX0=
-->