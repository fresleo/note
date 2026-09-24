
几个定义的函数名称：
makeEditorInfo：KZ_METACLASS_END() 这个宏里硬编码调用了
 
create：KZ_METACLASS_BEGIN/END 让类注册到 Kanzi 的 metaclass 系统运行时创建对象时，Kanzi 会Metaclass::create(...)

initialize

定义智能指针


**1 定义公共属性**

例如：
//定义公共属性  
static PropertyType<Vector3> SimulationAreaSizeProperty;  
static PropertyType<Vector3> SimulationAreaCenterProperty;  
static PropertyType<float> BoundaryForceProperty;  
static PropertyType<float> BoundaryMarginProperty;  
static kanzi::PropertyType<float> TimeScaleProperty;  
static kanzi::PropertyType<bool> ShowBoundsProperty;  
static kanzi::PropertyType<bool> SimulationEnabledProperty;


**2.定义宏数据——hpp**

KZ_METACLASS_BEGIN(TestWorldNode,Node3D,"TestWorldNode")  
    KZ_METACLASS_PROPERTY_TYPE(SimulationAreaSizeProperty)  
    KZ_METACLASS_PROPERTY_TYPE(SimulationAreaCenterProperty)  
    KZ_METACLASS_PROPERTY_TYPE(BoundaryForceProperty)  
    KZ_METACLASS_PROPERTY_TYPE(BoundaryMarginProperty)  
    KZ_METACLASS_PROPERTY_TYPE(TimeScaleProperty)  
    KZ_METACLASS_PROPERTY_TYPE(ShowBoundsProperty)  
    KZ_METACLASS_PROPERTY_TYPE(SimulationEnabledProperty)  
KZ_METACLASS_END()


**3.描述studio如何进行编辑当前元数据**

static kanzi::PropertyTypeEditorInfoSharedPtr makeEditorInfo();

**4.实现定义的propertytype：**

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

**5.实现create：**

BoidsWorldNodeSharedPtr BoidsWorldNode::create(Domain* domain, string_view name)  
{  
    auto node = BoidsWorldNodeSharedPtr(new BoidsWorldNode(domain, name));  
    node->initialize();  
    return node;  
}

**6实现构造函数**

TestWorldNode :: TestWorldNode(Domain* domain, string_view name):  
    Node3D(domain, name)  
{  
}

**7：实现initialize函数**

void TestWorldNode :: initialize()  
{  
    Node3D :: initialize();  
}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzc1NTA4NSw4NTAyOTQxMjQsMjIwNT
Q0OTM1LC0xMTcwNTg4NDMxLDYxODQzNTUzNl19
-->