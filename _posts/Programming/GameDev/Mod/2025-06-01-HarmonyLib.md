---
categories: something
---

# Harmony

每次使用都要创建一个实例. `var harmony = new Harmony("yourmod");`

## 补丁

- 前缀 Prefix

  > 前缀方法在原始方法之前执行，返回false则跳过原始方法。void 和 true
  > 则继续执行原方法

- 后缀 Postfix

  > 后缀方法在原始方法之后执行。

- 转接器 Transpiler

  > 转接器用于修改原始方法的 IL (中间语言) 代码。它通常需要对 IL
  > 有深入的了解

在方法上标记:

    //重载方法的补丁
    //无参,可以直接写
    [HarmonyPatch(typeof(People), "Say")]
    [HarmonyPatch(typeof(People), "Say", new Type[] { })]
    //一个string参数
    [HarmonyPatch(typeof(People), "Say", new Type[] { typeof(string) })]
    //一个int参数
    [HarmonyPatch(typeof(People), "Say", new Type[] { typeof(int) })]


    //属性的Getter
    [HarmonyPatch(typeof(People), "Name", MethodType.Getter]
    //属性的Setter
    [HarmonyPatch(typeof(People), "Age", MethodType.Setter]
    //构造函数
    [HarmonyPatch(typeof(People), MethodType.Constructor]

`__result` : 获得原来方法的返回值:

    [HarmonyPatch(typeof(People), "Name",  MethodType.Getter)]
    class PeopleNamePatch
    {
        public static bool Prefix(ref string __result)
        {
            __result = "张三";
            return false; //拦截原方法，直接使用我们给出的结果
        }
    }

`__instance` : 如果原方法不是静态方法，获得其实例:

    [HarmonyPatch(typeof(People), "Sleep")]
    class PeopleSleepPatch
    {
        public static void Postfix(People __instance)
        {
            Console.WriteLine(__instance.Name + "睡觉了");
        }
    }

用原方法中的同名参数来访问对应的参数，如果不是引用类型要使用ref

在 `SubModule` 应用补丁 `harmony.PatchAll();`

\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~ 取之
B站宵夜97
