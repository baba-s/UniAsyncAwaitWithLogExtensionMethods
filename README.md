# UniAsyncAwaitWithLogExtensionMethods

async / await で処理の開始時と終了時にログを出力できる拡張メソッド

## 使用例

```cs
using Kogane;
using System.Threading.Tasks;
using UnityEngine;

public sealed class Example : MonoBehaviour
{
    private async void Start()
    {
#if ENABLE_RELEASE
        AsyncAwaitWithLogExtensionMethods.OnStartLog      = null;
        AsyncAwaitWithLogExtensionMethods.OnFinishLog     = null;
        AsyncAwaitWithLogExtensionMethods.OnStartTimeLog  = null;
        AsyncAwaitWithLogExtensionMethods.OnFinishTimeLog = null;
#else
        AsyncAwaitWithLogExtensionMethods.OnStartLog      = message => Debug.Log( $"{message} 開始" );
        AsyncAwaitWithLogExtensionMethods.OnFinishLog     = message => Debug.Log( $"{message} 終了" );
        AsyncAwaitWithLogExtensionMethods.OnStartTimeLog  = message => Debug.Log( $"{message} 開始" );
        AsyncAwaitWithLogExtensionMethods.OnFinishTimeLog = ( message, elapsed ) => Debug.Log( $"{message} 終了 {elapsed.TotalSeconds} 秒" );
#endif

        await Test1().WithLog( "Test1" );

        var str1 = await Test2().WithLog( "Test2" );

        await Test1().WithTimeLog( "Test1" );

        var str2 = await Test2().WithTimeLog( "Test2" );
    }

    private static async Task Test1()
    {
        await Task.Delay( 1000 );
    }

    private static async Task<string> Test2()
    {
        await Task.Delay( 1000 );
        return "ピカチュウ";
    }
}
```