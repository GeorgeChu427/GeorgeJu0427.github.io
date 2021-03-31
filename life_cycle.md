## Android Activity LifeCycle

| Method    | Decription                                                 |
| --------- | ---------------------------------------------------------- |
| onCreate  | called when activity is first created.                     |
| onStart   | called when activity is becoming visible to the user.      |
| onResume  | called when activity will start interacting with the user. |
| onPause   | called when activity is not visible to the user.           |
| onStop    | called when activity is no longer visible to the user.     |
| onRestart | called after your activity is stopped, prior to start.     |
| onDestroy | called before the activity is destroyed.                   |

![](\photo\Android-Activity-Lifecycle.png)



### onCreate()
- 在系統首次創建Activity時觸發
- 整個Activity的生命週期中，只發生一次
- 常用於binding、viewModel建立


### onStart()
- 當Activity進入"已開始"狀態時回調
- 當調用時，使用者可看見Activity

### onResume()
- Activity會在進入"已恢復"狀態時回調


### onPause()
- 表示Activity不再位於前景 (盡管在多視窗模式時Activity可被看見)


### onStop()
如果您的 Activity 不再對用戶可見，說明其已進入“已停止”狀態，因此系統將調用 onStop() 回調。例如，當新啟動的 Activity 覆蓋整個屏幕時，可能會發生這種情況。如果 Activity 已結束運行並即將終止，系統還可以調用 onStop()。

當 Activity 進入已停止狀態時，與 Activity 生命週期相關聯的所有生命週期感知型組件都將收到 ON_STOP 事件。這時，生命週期組件可以停止在組件未顯示在屏幕上時無需運行的任何功能。

在 onStop() 方法中，應用應釋放或調整在應用對用戶不可見時的無用資源。例如，應用可以暫停動畫效果，或從精確位置更新切換到粗略位置更新。使用 onStop() 而非 onPause() 可確保與界面相關的工作繼續進行，即使用戶在多窗口模式下查看您的 Activity 也能如此。

您還應使用 onStop() 執行 CPU 相對密集的關閉操作。例如，如果您無法找到更合適的時機來將信息保存到數據庫，可以在 onStop() 期間執行此操作。以下示例展示了 onStop() 的實現，它將草稿筆記內容保存到持久性存儲空間中：

當您的 Activity 進入“已停止”狀態時，Activity 對象會繼續駐留在內存中：該對象將維護所有狀態和成員信息，但不會附加到窗口管理器。 Activity 恢復後，Activity 會重新調用這些信息。您無需重新初始化在任何回調方法導致 Activity 進入“已恢復”狀態期間創建的組件。系統還會追踪佈局中每個 View 對象的當前狀態，如果用戶在 EditText 微件中輸入文本，系統將保留文本內容，因此您無需保存和恢復文本。


### onDestroy()
銷毀 Ativity 之前，系統會先調用 onDestroy()。系統調用此回調的原因如下：

Activity 即將結束（由於用戶徹底關閉 Activity 或由於系統為 Activity 調用 finish()），或者
由於配置變更（例如設備旋轉或多窗口模式），系統暫時銷毀 Activity
當 Activity 進入已銷毀狀態時，與 Activity 生命週期相關聯的所有生命週期感知型組件都將收到 ON_DESTROY 事件。這時，生命週期組件可以在 Activity 被銷毀之前清理所需的任何數據。

您應使用 ViewModel 對象來包含 Activity 的相關視圖數據，而不是在您的 Activity 中加入邏輯來確定 Activity 被銷毀的原因。如果因配置變更而重新創建 Activity，ViewModel 不必執行任何操作，因為系統將保留 ViewModel 並將其提供給下一個 Activity 實例。如果不重新創建 Activity，ViewModel 將調用 onCleared() 方法，以便在 Activity 被銷毀前清除所需的任何數據。

您可以使用 isFinishing() 方法區分這兩種情況。

如果 Activity 即將結束，onDestroy() 是 Activity 收到的最後一個生命週期回調。如果由於配置變更而調用 onDestroy()，系統會立即新建 Activity 實例，然後在新配置中為新實例調用 onCreate()。

onDestroy() 回調應釋放先前的回調（例如 onStop()）尚未釋放的所有資源。



### ActvityA to ActivityB
pauseA -> createB -> startB -> resumeB -> stopA