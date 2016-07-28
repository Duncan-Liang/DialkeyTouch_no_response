按back键退出cmb界面之后，查看跟cmb相关的进程可以看到如下：
USER      PID   PPID  VSIZE  RSS   WCHAN              PC  NAME
u0_a100   11102 403   745632 61352 SyS_epoll_ 00f7484d24 S cmb.pb:push
u0_a100   11151 403   743456 63708 SyS_epoll_ 00f7484d24 S cmb.pb:pushservice
u0_a100   11192 403   769980 71176 SyS_epoll_ 00f7484d24 S cmb.pb:CMCoreService
从dumpsys windows的信息来看：
Window #5 Window{ff87051 u0 cmb.pb}:

    mDisplayId=0 stackId=4 mSession=Session{8c87fea 11192:u0a10100} mClient=android.os.BinderProxy@11f5178

    mOwnerUid=10100 mShowToOwnerOnly=true package=cmb.pb appop=SYSTEM_ALERT_WINDOW

    mAttrs=WM.LayoutParams{(0,0)(wrapxwrap) gr=#11 sim=#20 ty=2003 fl=#1000008 fmt=-3}

    Requested w=240 h=41 mLayoutSeq=786

    mHasSurface=true mShownFrame=[240.0,595.0][480.0,636.0] isReadyForDisplay()=true

    WindowStateAnimator{994e1b7 }:

    Surface: shown=true layer=111000 alpha=1.0 rect=(240.0,595.0) 240.0 x 41.0
	  
可以发现window #5的对应的PID为11192。尝试kill cmb.pb:CMCoreService此进程再查看数据。
在最近程序里面删除招商银行界面，再查看跟cmb相关的进程如下：
USER      PID   PPID  VSIZE  RSS   WCHAN              PC  NAME
u0_a100   14308 403   744848 60984 SyS_epoll_ 00f7484d24 S cmb.pb:push
在设置应用中，找到招商银行，选择强行停止以后。终端中就没有跟cmb相关的进程了。



	  