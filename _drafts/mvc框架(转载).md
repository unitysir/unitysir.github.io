```C#
**********************************Model数据管理***************************************************

using UnityEngine;
using System.Collections;

/// <summary>
/// 模型委托（当用户信息发生变化时执行）
/// </summary>
public delegate void OnValueChange (int val);

public class PlayerMsgModel
{
    //玩家等级
    private int playerLevel;
    //玩家经验
    private int playerExperience;
    //玩家升级经验
    private int playerFullExperience;
    //金币数量
    private int goldNum;
    //声明委托对象，接收当等级发生变化时，触发的事件
    public OnValueChange OnLevelChange;
    //声明委托对象，接收当经验发生变化时，触发的事件
    public OnValueChange OnExperienceChange;
    //声明委托对象，接收当升级经验发生变化时，触发的事件
    public OnValueChange OnFullExperienceChange;
    //声明委托对象，接收当金币数量发生变化时，触发的事件
    public OnValueChange OnGoldNumChange;

    //单例
    private static PlayerMsgModel mod;

    public static PlayerMsgModel GetMod ()
    {
        if (mod == null) {
            mod = new PlayerMsgModel ();
        }
        return mod;
    }

    private PlayerMsgModel ()
    {
    }

    /// <summary>
    /// 玩家等级属性
    /// </summary>
    /// <value>The player level.</value>
    public int PlayerLevel {
        get {
            return playerLevel;
        }
        set {
            playerLevel = value;
            //如果委托对象不为空
            if (OnLevelChange != null) {
                //执行委托
                OnLevelChange (playerLevel);
            }
        }
    }

    /// <summary>
    /// 玩家经验属性
    /// </summary>
    /// <value>The player experience.</value>
    public int PlayerExperience {
        get {
            return playerExperience;
        }
        set {
            playerExperience = value;
            if (OnExperienceChange != null) {
                OnExperienceChange (playerExperience);
            }
        }
    }

    /// <summary>
    /// 玩家升级经验属性
    /// </summary>
    /// <value>The player full experience.</value>
    public int PlayerFullExperience {
        get {
            return playerFullExperience;
        }
        set {
            playerFullExperience = value;
            if (OnFullExperienceChange != null) {
                OnFullExperienceChange (playerFullExperience);
            }
        }
    }

    /// <summary>
    /// 金币数量属性
    /// </summary>
    /// <value>The gold number.</value>
    public int GoldNum {
        get {
            return goldNum;
        }
        set {
            goldNum = value;
            if (OnGoldNumChange != null) {
                OnGoldNumChange (goldNum);
            }
        }
    }
}

**********************************View显示管理管理***************************************************

using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class PlayerMsgView : MonoBehaviour
{
    //UI
    public Text playerLevel;
    public Text playerExperience;
    public Text goldNum;
    public Button experienceUpButton;

    void Start ()
    {
        //委托事件绑定
        PlayerMsgModel.GetMod ().OnLevelChange += SetLevel;
        //委托事件绑定
        PlayerMsgModel.GetMod ().OnExperienceChange += SetExperience;

        PlayerMsgModel.GetMod ().OnFullExperienceChange += SetFullExperience;
        PlayerMsgModel.GetMod ().OnGoldNumChange += SetGoldNum;
        //View绑定按钮控制功能
        experienceUpButton.onClick.AddListener (
            PlayerMsgController.controller.OnExperienceUpButtonClick);
    }

    //修改UILevel值
    public void SetLevel (int level)
    {
        playerLevel.text = level.ToString ();
    }

    //修改UI经验值
    public void SetExperience (int experience)
    {
        //将字符串以“/”拆开
        string[] str = playerExperience.text.Split (new char []{ '/' });
        //用新的经验值重组
        playerExperience.text = experience + "/" + str [1];
    }

    public void SetFullExperience (int fullExiperience)
    {
        string[] str = playerExperience.text.Split (new char []{ '/' });


        playerExperience.text = str [0] + "/" + fullExiperience;
    }

    public void SetGoldNum (int goldn)
    {
        goldNum.text = goldn.ToString ();
    }
}

**********************************Controller控制代码管理***************************************************

using UnityEngine;
using System.Collections;

public class PlayerMsgController : MonoBehaviour
{
    public static PlayerMsgController controller;

    private int levelUpValue = 20;

    void Awake ()
    {
        controller = this;
    }

    void Start ()
    {
        PlayerMsgModel.GetMod ().PlayerLevel = 1;
        PlayerMsgModel.GetMod ().PlayerExperience = 0;
        PlayerMsgModel.GetMod ().PlayerFullExperience = 100;
        PlayerMsgModel.GetMod ().GoldNum = 0;
    }

    /// <summary>
    /// 提升经验按钮点击事件
    /// </summary>
    public void OnExperienceUpButtonClick ()
    {
        PlayerMsgModel.GetMod ().PlayerExperience += levelUpValue;
        if (PlayerMsgModel.GetMod ().PlayerExperience
            >= PlayerMsgModel.GetMod ().PlayerFullExperience) {
            PlayerMsgModel.GetMod ().PlayerLevel += 1;
            PlayerMsgModel.GetMod ().PlayerFullExperience +=
                200 * PlayerMsgModel.GetMod ().PlayerLevel;
            levelUpValue += 20;
            if (PlayerMsgModel.GetMod ().PlayerLevel % 3 == 0) {
                PlayerMsgModel.GetMod ().GoldNum +=
                    100 * PlayerMsgModel.GetMod ().PlayerLevel;
            }
        }
    }
}
```

