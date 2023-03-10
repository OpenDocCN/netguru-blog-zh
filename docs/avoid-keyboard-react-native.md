# 像专业人士一样避免使用键盘

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/avoid-keyboard-react-native>

 当构建具有不同形式和输入的功能性和高性能的移动 ui 时，开发人员不得不与覆盖屏幕底部的软键盘这一常见问题作斗争，除非键盘被隐藏，否则用户无法触及这些软键盘。

在每个项目中，开发人员必须将文本输入合并到不同类型的表单、底部表单或模态中，并且在显示软键盘时不会破坏这些布局。此外，在编写 React 原生应用程序时，开发人员必须考虑如何以一种在两个平台上产生一致行为的方式来解决这个问题。有几个解决方案可以用来实现最佳效果，从内置开始，通过第三方，到为特定用例编写的完全自定义的逻辑。本文将在不同的测试案例中描述和比较它们。但在此之前，让我们检查一下如何在原生 Android 和 iOS 项目中处理键盘问题。

Android 有一个内置的[Android:windowSoftInputMode](http://web.archive.org/web/20230221160518/https://developer.android.com/guide/topics/manifest/activity-element#wsoft)活动参数，它控制键盘如何与活动的 UI 一起显示。在 iOS 上，没有内置的方式。开发人员转而依赖一些定制逻辑或流行的 [IQKeyboardManager](http://web.archive.org/web/20230221160518/https://github.com/hackiftekhar/IQKeyboardManager) 库。这些(原生)解决方案可以在 React 原生项目中使用——在 Android 上设置清单文件中的值，并为 iOS 安装`react-native-keyboard-manager`，这是`IQKeyboardManager`的 RN 包装器。

现在，让我们来看看如何在 React Native 中实现它:

## 避免键盘解决方案

### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

`KeyboardAvoidingView`是 React 原生内置组件，完全 JS 实现。它依赖于 RN 的键盘事件(iOS 上的`keyboardWillChangeFrame`&Android 上的`keyboardDidHide/Show`)，并基于提供的`behavior`道具，应用额外的填充、转换或更改容器的高度。

### `react-native-keyboard-aware-scroll-view` + `android:windowSoftInputMode=”adjustPan”`

`react-native-keyboard-aware-scroll-view`是一个具有完整 JS 实现的库，它提供了一个增强的`ScrollView`组件，通过对当前聚焦的输入应用底部填充和滚动来对键盘事件做出反应。它还提供了`FlatList` & `SectionList`的实现。它还公开了`listenToKeyboardEvents` HOC，可以用来包装定制的滚动组件。

### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

`react-native-keyboard-manager`是 iOS `IQKeyboardManager`的包装器库。这是 Swift & Objective-C 项目中使用的“首选”解决方案。它对正确的容器应用填充或转换，还提供了软键盘外观定制的方法。

### `react-native-avoid-softinput`

`react-native-avoid-softinput`是一个具有本机实现的库。它公开了本机模块，该模块可以全局地对键盘事件做出反应，并应用填充或转换来对根视图做出反应。它还包含原生组件，用于包装应该推到键盘上方的内容。

## 测试案例

为了比较，将使用以下示例:

*   表单示例-带有不同高度的文本输入(单行和多行)的屏幕
*   带有 [@gorhom/bottomsheet](http://web.archive.org/web/20230221160518/https://github.com/gorhom/react-native-bottom-sheet) lib -屏幕的底部表单示例，底部表单包含单个文本字段和确认按钮
*   带有 RN `Modal`组件的模态表单示例-带有包含文本输入(单行&多行)的模态的屏幕，这些文本输入具有不同的高度
*   带有 RN `Modal`组件的底部表单模式示例-带有包含单个文本字段和确认按钮的模式的屏幕
*   带有[@ gor hom/Portal](http://web.archive.org/web/20230221160518/https://github.com/gorhom/react-native-portal)lib-screen 的 portal 表单示例，包含不同高度的文本输入(单行&多行)
*   多输入示例-在可滚动组件中放置多个文本字段的屏幕

所有示例均可在[测试报告](http://web.archive.org/web/20230221160518/https://github.com/mateusz1913/KeyboardManagerOverview)中找到。

### 例如

让我们从最流行的用例开始。

```
 const FormExample: React.FC = () => {
  //...

  return <>
    <View style={styles.logoContainer}>
      <Image
        resizeMode="contain"
        source={ { uri: 'https://reactnative.dev/img/tiny_logo.png' } }
        style={styles.logo}
      />
    </View>
    <View style={styles.inputsContainer}>
      <TextInput
        placeholder="Single line input"
        style={styles.input}
      />
      <TextInput
        multiline
        placeholder="Multiline input"
        style={[ styles.input, styles.multilineInput ]}
      />
      <TextInput
        multiline
        placeholder="Large multiline input"
        style={[ 
          styles.input,
          styles.multilineInput,
          styles.largeMultilineInput
        ]}
      />
    </View>
    <View style={styles.submitButtonContainer}>
      <Button
        onPress={() => {
          // On submit ...
        }}
        title="Submit"
      />
    </View>
  </>;
}; 
```

表单组件将被全屏滚动组件包裹(默认为`ScrollView`)。

要认为解决方案是成功的，它应该处理:

*   应用底部填充或平移
*   将文本栏推到键盘上方，但低于屏幕的上边缘

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAvoidingViewFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <KeyboardAvoidingView
      behavior="position"
      style={styles.keyboardAvoidingView}>
      <ScrollView
        bounces={false}
        contentContainerStyle={commonStyles.scrollContainer}
        contentInsetAdjustmentBehavior="always"
        overScrollMode="always"
        showsVerticalScrollIndicator={true}
        style={commonStyles.scroll}
      >
        <FormExample />
      </ScrollView>
    </KeyboardAvoidingView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-aware-scroll-view` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAwareScrollViewFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <KeyboardAwareScrollView
      enableOnAndroid={true}
      enableResetScrollToCoords={false}
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <FormExample />
    </KeyboardAwareScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <FormExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModuleFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    AvoidSoftInput.setEnabled(true);
    return () => {
      AvoidSoftInput.setEnabled(false);
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <FormExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const AvoidSoftInputViewFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <AvoidSoftInputView style={styles.avoidSoftInputView}>
      <ScrollView
        bounces={false}
        contentContainerStyle={commonStyles.scrollContainer}
        contentInsetAdjustmentBehavior="always"
        overScrollMode="always"
        showsVerticalScrollIndicator={true}
        style={commonStyles.scroll}>
        <FormExample />
      </ScrollView>
    </AvoidSoftInputView>
  </SafeAreaView>;
}; 
```

#### 结果:

在有`KeyboardAvoidingView`的屏幕上，当键盘弹出时，屏幕的顶部和底部被夹住，所以“提交”按钮是不可访问的。此外，当多行输入被聚焦时，即使它不会被软键盘覆盖，它也会被推高。

在 iOS 上的`react-native-keyboard-aware-scroll-view`屏幕中，选择第一个输入后，将其推到键盘上方。当选择大输入时，它位于可视区域顶部边缘的稍上方，但其余内容(在屏幕的顶部和底部)很容易访问。在 Android 上，屏幕的底部被稍微修剪了一下，但当键盘显示出来时，输入会被正确地推到上面，而且只有在需要的时候。

带有`react-native-keyboard-manager`和`react-native-avoid-softinput`的屏幕以类似的效果处理键盘——单行输入的底边仅在需要时被推到键盘上方，当聚焦大量输入时，它仅被推到`ScrollView`顶边的顶部。只要键盘可见，用户就可以滚动到屏幕的最底部。

### 底部表单示例

在底部表单示例中，将使用来自`@gorhom/bottom-sheet`库的`BottomSheetModal`组件。一个例子将接受`BottomSheetWrapper`属性来包装应该显示在键盘上方的内容(默认包装器是简单的`View`组件)。

```
 const SNAP_POINTS = [ 'CONTENT_HEIGHT' ];

const Backdrop: React.FC = () => <View style={styles.backdrop} />;

const DefaultBottomSheetWrapper: React.FC = ({ children }) => <View 
  style={styles.container}>
  {children}
</View>;

interface Props {
  BottomSheetWrapper?: React.FC
}

const BottomSheetExample: React.FC<Props> = ({
  BottomSheetWrapper = DefaultBottomSheetWrapper
}) => {
  //...

  return <View style={commonStyles.screenContainer}>
    <Button
      onPress={presentBottomSheet}
      title="Open bottom sheet"
    />
    <BottomSheetModal
      ref={bottomSheetModalRef}
      backdropComponent={Backdrop}
      contentHeight={animatedContentHeight}
      enableDismissOnClose
      enablePanDownToClose
      handleHeight={animatedHandleHeight}
      index={0}
      snapPoints={animatedSnapPoints}
    >
      <BottomSheetView
        onLayout={handleContentLayout}
        style={styles.container}>
        <SafeAreaView edges={[ 'bottom' ]} style={styles.container}>
          <BottomSheetWrapper>
            <Text style={styles.header}>Bottom sheet header</Text>
            <TextInput style={styles.input} />
            <View style={styles.buttonContainer}>
              <Button
                onPress={dismissBottomSheet}
                title="Confirm"
              />
            </View>
          </BottomSheetWrapper>
        </SafeAreaView>
      </BottomSheetView>
    </BottomSheetModal>
  </View>;
}; 
```

在这个测试用例中，没有使用滚动组件，所以`react-native-keyboard-aware-scroll-view`不会被检查。

要认为解决方案是成功的，它应该处理:

*   将整个底部表单内容推到键盘的上边缘上方

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const BottomSheetWrapper: React.FC = ({ children }) => {
  return <KeyboardAvoidingView
    behavior="position"
    contentContainerStyle={styles.keyboardAvoidingView}
    style={styles.keyboardAvoidingView}>
    {children}
  </KeyboardAvoidingView>;
};

const KeyboardAvoidingViewBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}
    >
      <BottomSheetExample BottomSheetWrapper={BottomSheetWrapper} />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
      RNKeyboardManager.setKeyboardDistanceFromTextField(100);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
        // Default value
        RNKeyboardManager.setKeyboardDistanceFromTextField(10);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <BottomSheetExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModuleBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    AvoidSoftInput.setEnabled(true);
    AvoidSoftInput.setAvoidOffset(100);
    return () => {
      AvoidSoftInput.setEnabled(false);
      AvoidSoftInput.setDefaultAppSoftInputMode();
      AvoidSoftInput.setAvoidOffset(0); // Default value
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <BottomSheetExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const BottomSheetWrapper: React.FC = ({ children }) => {
  return <AvoidSoftInputView style={styles.avoidSoftInputView}>
    {children}
  </AvoidSoftInputView>;
};

const AvoidSoftInputViewBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <BottomSheetExample BottomSheetWrapper={BottomSheetWrapper} />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### 结果:

在`KeyboardAvoidingView`屏幕中，打开底部表单并聚焦输入后，内容不会改变其位置，这使其被键盘覆盖。

在带有`react-native-keyboard-manager`和`react-native-avoid-softinput`“模块”的屏幕中，当示例文本字段被聚焦时，整个底部表单被推到键盘上方。当底部表单被关闭时，键盘消失，屏幕转到开始位置。

在带有`react-native-avoid-softinput`“组件”的屏幕上，当键盘弹出时，底部的工作表保持在原来的位置，然而，内容被翻译到键盘上边缘的上方，使其被夹住，用户看不见。

### 模态形式示例

本示例将使用核心的反应原生`Modal`组件。一个示例将接受`ModalContentWrapper`作为道具，然后将包装以模态显示的内容，并在键盘显示时应用翻译或填充(默认包装将使用`ScrollView`)。

```
 const DefaultModalContentWrapper: React.FC = ({ children }) => <View
  style={styles.wrapper}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</View>;

interface Props {
  ModalContentWrapper?: React.FC
}

const ModalFormExample: React.FC<Props> = ({
  ModalContentWrapper = DefaultModalContentWrapper
}) => {
  //...

  return <View style={commonStyles.screenContainer}>
    <Button
      onPress={openModal}
      title="Open modal"
    />
    <RNModal
      animationType="slide"
      onRequestClose={closeModal}
      statusBarTranslucent={true}
      style={styles.modal}
      supportedOrientations={[ 'landscape', 'portrait' ]}
      transparent={true}
      visible={isModalVisible}
    >
      <SafeAreaView
        edges={[ 'bottom', 'left', 'right' ]}
        style={styles.modalContent}>
        <View style={styles.container}>
          <View style={styles.wrapper}>
            <CloseButton onPress={closeModal} />
          </View>
          <ModalContentWrapper>
            <View style={styles.spacer} />
            <View style={styles.inputsContainer}>
              <TextInput
                placeholder="Single line input"
                style={styles.input}
              />
              <TextInput
                multiline
                placeholder="Multiline input"
                style={[ styles.input, styles.multilineInput ]}
              />
            </View>
          </ModalContentWrapper>
        </View>
      </SafeAreaView>
    </RNModal>
  </View>;
}; 
```

要认为解决方案是成功的，它应该处理:

*   应用底部填充或平移
*   在键盘上方推动文本输入

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAvoidingViewModalContentWrapper: React.FC = ({
  children
}) => <KeyboardAvoidingView
  behavior="position"
  contentContainerStyle={styles.keyboardAvoidingView}
  style={styles.keyboardAvoidingView}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</KeyboardAvoidingView>;

const KeyboardAvoidingViewModalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ModalFormExample
      ModalContentWrapper={KeyboardAvoidingViewModalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-aware-scroll-view` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAwareScrollViewModalContentWrapper: React.FC = ({
  children
}) => <KeyboardAwareScrollView
  enableOnAndroid={true}
  enableResetScrollToCoords={false}
  bounces={false}
  contentContainerStyle={commonStyles.scrollContainer}
  contentInsetAdjustmentBehavior="always"
  overScrollMode="always"
  showsVerticalScrollIndicator={true}
  style={commonStyles.scroll}>
  {children}
</KeyboardAwareScrollView>;

const KeyboardAwareScrollViewModalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ModalFormExample
      ModalContentWrapper={KeyboardAwareScrollViewModalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerModalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ModalFormExample />
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModuleModalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ModalFormExample />
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const AvoidSoftInputViewModalContentWrapper: React.FC = ({
  children
}) => <AvoidSoftInputView
  style={styles.avoidSoftInput}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</AvoidSoftInputView>;

const AvoidSoftInputViewModalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ModalFormExample
      ModalContentWrapper={AvoidSoftInputViewModalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### 结果:

在有`KeyboardAvoidingView`的屏幕上，当输入被聚焦时，在键盘上方被轻微推动，但第二次输入的底部被键盘部分覆盖，无法滚动。

在 iOS 上带有`react-native-keyboard-aware-scroll-view`、iOS 上带有`react-native-keyboard-manager`和`react-native-avoid-softinput`“组件”的屏幕以类似的方式处理键盘回避——在聚焦其中一个文本字段后，表单被推到键盘的上边缘，用户可以滚动到模态内容的最底部。

在 Android 上，`react-native-keyboard-aware-scroll-view`当第二个输入被聚焦时，屏幕会截取它的底部。

在 Android 上，显示键盘时，带有`android:windowSoftInputMode="adjustResize"`的屏幕不会上移。这是因为 React Native `Modal`在幕后使用 Android `Dialog`控件，它有自己的`android:windowSoftInputMode`属性，不幸的是，不能从 JS 代码中设置。

`react-native-avoid-softinput`“模块”对 RN `Modal`组件不起作用——当键盘弹出时，表单停留在原来的位置，键盘下的内容无法访问。

### 底板模态示例

下一个例子也使用 React Native `Modal`组件，这一次，它将它的子组件显示为底部表单。与前面的例子类似，它接受一个`ModalContentWrapper`属性(默认为`View`组件)。

```
 const DefaultModalContentWrapper: React.FC = ({
  children
}) => <View style={styles.wrapper}>
  {children}
</View>;

interface Props {
  ModalContentWrapper?: React.FC
}

const ModalBottomSheetExample: React.FC<Props> = ({
  ModalContentWrapper = DefaultModalContentWrapper
}) => {
  const [ isModalVisible, setIsModalVisible ] = useState(false);

  function closeModal() {
    setIsModalVisible(false);
  }

  function openModal() {
    setIsModalVisible(true);
  }

  return <View style={commonStyles.screenContainer}>
    <Button
      onPress={openModal}
      title="Open modal"
    />
    <RNModal
      animationType="slide"
      onRequestClose={closeModal}
      statusBarTranslucent={true}
      style={styles.modal}
      supportedOrientations={[ 'landscape', 'portrait' ]}
      transparent={true}
      visible={isModalVisible}
    >
      <SafeAreaView 
        edges={[ 'bottom', 'left', 'right' ]}
        style={styles.modalContent}>
        <Pressable onPress={closeModal} style={styles.modalContent}>
          <ModalContentWrapper>
            <View style={styles.container}>
              <Text style={styles.header}>Bottom sheet header</Text>
              <TextInput style={styles.input} />
              <View style={styles.buttonContainer}>
                <Button
                  onPress={closeModal}
                  title="Confirm"
                />
              </View>
            </View>
          </ModalContentWrapper>
        </Pressable>
      </SafeAreaView>
    </RNModal>
  </View>;
}; 
```

与常规底部表单情况一样，`react-native-keyboard-aware-scroll-view`将不被检查，因为示例中没有使用滚动组件。

要认为解决方案是成功的，它应该处理:

*   将整个底部表单内容推到键盘的上边缘上方

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const BottomSheetModalContentWrapper: React.FC = ({ children }) => {
  return <KeyboardAvoidingView
    behavior="position"
    contentContainerStyle={styles.keyboardAvoidingView}
    style={styles.keyboardAvoidingView}>
    {children}
  </KeyboardAvoidingView>;
};

const KeyboardAvoidingViewModalBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}
    >
      <ModalBottomSheetExample
        ModalContentWrapper={BottomSheetModalContentWrapper}
      />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerModalBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
      RNKeyboardManager.setKeyboardDistanceFromTextField(100);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
        // Default value
        RNKeyboardManager.setKeyboardDistanceFromTextField(10);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <ModalBottomSheetExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModuleModalBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    AvoidSoftInput.setEnabled(true);
    AvoidSoftInput.setAvoidOffset(100);
    return () => {
      AvoidSoftInput.setEnabled(false);
      AvoidSoftInput.setDefaultAppSoftInputMode();
      AvoidSoftInput.setAvoidOffset(0); // Default value
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <ModalBottomSheetExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const BottomSheetModalContentWrapper: React.FC = ({ children }) => {
  return <AvoidSoftInputView style={styles.avoidSoftInputView}>
    {children}
  </AvoidSoftInputView>;
};

const AvoidSoftInputViewModalBottomSheetScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <ModalBottomSheetExample
        ModalContentWrapper={BottomSheetModalContentWrapper}
      />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### 结果:

在 iOS 上带有`KeyboardAvoidingView`、`react-native-keyboard-manager`和`react-native-avoid-softinput`“组件”的屏幕中，当显示键盘时，整个底部表单被推到键盘上方，其内容易于访问。

和前面的例子一样，带有`android:windowSoftInputMode="adjustResize"`属性和`react-native-avoid-softinput`“模块”屏幕的 Android 屏幕不会处理显示的键盘。

### 门户表单示例

下一个例子将使用来自`@gorhom/portal`库的`Portal`组件。它接受`PortalContentWrapper` prop，和前面的例子一样，它将包装显示的表单内容(默认使用`ScrollView`)。

```
 const DefaultPortalContentWrapper: React.FC = ({
  children
}) => <View style={styles.scrollWrapper}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</View>;

interface Props {
  PortalContentWrapper?: React.FC
}

/**
 * Remember to render it under `PortalProvider` from `@gorhom/portal` or
 * `BottomSheetModalProvider` from `@gorhom/bottom-sheet`
 */
const PortalFormExample: React.FC<Props> = ({
  PortalContentWrapper = DefaultPortalContentWrapper
}) => {
  //...

  return <View style={commonStyles.screenContainer}>
    <Button
      onPress={openPortal}
      title="Open portal"
    />
    {isPortalVisible ? <Portal>
      <View style={styles.portal}>
        <SafeAreaView
          edges={[ 'bottom', 'left', 'right' ]}
          style={styles.portalContent}>
          <View style={styles.container}>
            <View style={styles.wrapper}>
              <CloseButton onPress={closePortal} />
            </View>
            <PortalContentWrapper>
              <View style={styles.spacer} />
              <View style={styles.inputsContainer}>
                <TextInput
                  placeholder="Single line input"
                  style={styles.input}
                />
                <TextInput
                  multiline
                  placeholder="Multiline input"
                  style={[ styles.input, styles.multilineInput ]}
                />
              </View>
            </PortalContentWrapper>
          </View>
        </SafeAreaView>
      </View>
    </Portal> : null}
  </View>;
}; 
```

要认为解决方案是成功的，它应该处理:

*   应用底部填充或平移
*   在键盘上方推动输入

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAvoidingViewPortalContentWrapper: React.FC = ({
  children
}) => <KeyboardAvoidingView
  behavior="position"
  contentContainerStyle={styles.keyboardAvoidingView}
  style={styles.keyboardAvoidingView}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</KeyboardAvoidingView>;

const KeyboardAvoidingViewPortalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <PortalFormExample
      PortalContentWrapper={KeyboardAvoidingViewPortalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-aware-scroll-view` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAwareScrollViewPortalContentWrapper: React.FC = ({
  children
}) => <KeyboardAwareScrollView
  enableOnAndroid={true}
  enableResetScrollToCoords={false}
  bounces={false}
  contentContainerStyle={commonStyles.scrollContainer}
  contentInsetAdjustmentBehavior="always"
  overScrollMode="always"
  showsVerticalScrollIndicator={true}
  style={commonStyles.scroll}>
  {children}
</KeyboardAwareScrollView>;

const KeyboardAwareScrollViewPortalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <PortalFormExample
      PortalContentWrapper={KeyboardAwareScrollViewPortalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerPortalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <PortalFormExample />
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModulePortalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <PortalFormExample />
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const AvoidSoftInputViewPortalContentWrapper: React.FC = ({
  children
}) => <AvoidSoftInputView
  style={styles.avoidSoftInput}>
  <ScrollView
    bounces={false}
    contentContainerStyle={commonStyles.scrollContainer}
    contentInsetAdjustmentBehavior="always"
    overScrollMode="always"
    showsVerticalScrollIndicator={true}
    style={commonStyles.scroll}
  >
    {children}
  </ScrollView>
</AvoidSoftInputView>;

const AvoidSoftInputViewPortalScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <PortalFormExample
      PortalContentWrapper={AvoidSoftInputViewPortalContentWrapper}
    />
  </SafeAreaView>;
}; 
```

#### 结果:

iOS 上带有`KeyboardAvoidingView`、`react-native-keyboard-aware-scroll-view`、`react-native-keyboard-manager`和`react-native-avoid-softinput`“组件”的屏幕将与模态表单示例中的行为相同

与模态表单示例相反，Android 上带有`android:windowSoftInputMode="adjustResize"`的屏幕将匹配其在 iOS 上的行为。

`react-native-avoid-softinput`“模块”屏幕的行为与模态表单示例中的行为相同。

### 多输入示例

让我们以一个非常不寻常的例子来结束——文本字段的全屏列表。

```
 const MultipleInputsFormExample: React.FC = () => {
  return <>
    <TextInput placeholder="1" style={styles.input} />
    <TextInput placeholder="2" style={styles.input} />
    <TextInput placeholder="3" style={styles.input} />
    <TextInput placeholder="4" style={styles.input} />
    <TextInput placeholder="5" style={styles.input} />
    <TextInput placeholder="6" style={styles.input} />
    <TextInput placeholder="7" style={styles.input} />
    <TextInput placeholder="8" style={styles.input} />
    <TextInput placeholder="9" style={styles.input} />
    <TextInput placeholder="10" style={styles.input} />
    <TextInput placeholder="11" style={styles.input} />
    <TextInput placeholder="12" style={styles.input} />
    <TextInput placeholder="13" style={styles.input} />
  </>;
}; 
```

表单组件将被全屏滚动组件包裹(默认为`ScrollView`)。

要认为解决方案是成功的，它应该处理:

*   应用底部填充或平移
*   将焦点输入推到键盘上方，但低于可见屏幕的上边缘

#### `KeyboardAvoidingView` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAvoidingViewMultipleInputsFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <KeyboardAvoidingView
      behavior="position"
      style={styles.keyboardAvoidingView}>
      <ScrollView
        bounces={false}
        contentContainerStyle={commonStyles.scrollContainer}
        contentInsetAdjustmentBehavior="always"
        overScrollMode="always"
        showsVerticalScrollIndicator={true}
        style={commonStyles.scroll}
      >
        <MultipleInputsFormExample />
      </ScrollView>
    </KeyboardAvoidingView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-aware-scroll-view` + `android:windowSoftInputMode=”adjustPan”`

```
 const KeyboardAwareScrollViewMultipleInputsFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustPan();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <KeyboardAwareScrollView
      enableOnAndroid={true}
      enableResetScrollToCoords={false}
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <MultipleInputsFormExample />
    </KeyboardAwareScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-keyboard-manager` + `android:windowSoftInputMode=”adjustResize”`

```
 const IQKeyboardManagerMultipleInputsFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    if (Platform.OS !== 'ios') {
      AvoidSoftInput.setAdjustResize();
    } else {
      RNKeyboardManager.setEnable(true);
    }

    return () => {
      if (Platform.OS !== 'ios') {
        AvoidSoftInput.setDefaultAppSoftInputMode();
      } else {
        RNKeyboardManager.setEnable(false);
      }
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <MultipleInputsFormExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本机模块

```
 const AvoidSoftInputModuleMultipleInputsFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    AvoidSoftInput.setEnabled(true);
    return () => {
      AvoidSoftInput.setEnabled(false);
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <ScrollView
      bounces={false}
      contentContainerStyle={commonStyles.scrollContainer}
      contentInsetAdjustmentBehavior="always"
      overScrollMode="always"
      showsVerticalScrollIndicator={true}
      style={commonStyles.scroll}>
      <MultipleInputsFormExample />
    </ScrollView>
  </SafeAreaView>;
}; 
```

#### `react-native-avoid-softinput` -本地组件

```
 const AvoidSoftInputViewMultipleInputsFormScreen: React.FC = () => {
  const onFocusEffect = useCallback(() => {
    AvoidSoftInput.setAdjustNothing();
    return () => {
      AvoidSoftInput.setDefaultAppSoftInputMode();
    };
  }, []);

  useFocusEffect(onFocusEffect);

  return <SafeAreaView
    edges={[ 'bottom', 'left', 'right' ]}
    style={commonStyles.screenContainer}>
    <AvoidSoftInputView style={styles.avoidSoftInputView}>
      <ScrollView
        bounces={false}
        contentContainerStyle={commonStyles.scrollContainer}
        contentInsetAdjustmentBehavior="always"
        overScrollMode="always"
        showsVerticalScrollIndicator={true}
        style={commonStyles.scroll}>
        <MultipleInputsFormExample />
      </ScrollView>
    </AvoidSoftInputView>
  </SafeAreaView>;
}; 
```

#### 结果:

根据平台的不同，带有`KeyboardAvoidingView`的屏幕会有一些不同的行为。在 iOS 上，即使用户试图滚动到屏幕的最底部，最后一个文本栏也会被完全覆盖。在 Android 上，应用的填充量似乎是随机的。在这两个平台上，输入的位置都改变了，即使它没有被软键盘覆盖。

其余的屏幕以类似的方式处理显示的键盘——焦点输入仅在必要时在键盘上方滚动。当键盘可见时，可以访问全部内容。

## 摘要

很难在两个平台和不同的应用程序用例中实现一致的行为。通常情况下，比“我们把它包在`KeyboardAvoidingView`里吧”更复杂，它是内置的，所以应该可以用，不是吗？”。布局可以有不同的类型，有时，一个布局可以有需要单独处理的不同部分(例如，在可滚动组件内的表单以及位于屏幕底部和可滚动组件外的 CTA 按钮)。

使用`androidWindowSoftInputMode="adjustPan"`的`KeyboardAvoidingView`可以处理非常基本的表单和简单的基于模态的底层表单，但是它将很难处理更复杂的屏幕。使用`androidWindowSoftInputMode="adjustPan"`的`react-native-keyboard-aware-scroll-view`在具有可滚动内容的屏幕上工作得非常好，它在 Android 和 iOS 上的行为类似。`react-native-keyboard-manager`是 iOS 上的“即插即用”解决方案，可以处理所有测试用例，`androidWindowSoftInputMode="adjustResize"`覆盖了 Android 上的大部分场景。`react-native-avoid-softinput`在两个平台上给出一致的行为，并用其“模块”或“组件”处理所有测试用例。根据您的使用情况，所有这些解决方案都值得考虑。在一个项目中，一个“内置”的解决方案可以很好地工作，在其他项目中，它将需要引入一个以上的解决方案，包括一些自定义的解决方案，以实现所有布局的预期效果。