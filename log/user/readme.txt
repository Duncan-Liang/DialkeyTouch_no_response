[Current status]
目前我司部分手机概率出现按拨号盘键2，无法响应触摸事件的情况（触摸了，编辑框未出现数字2）。
附件提供对应log，请贵司查收，内部做了如下分析：
1，设置开发者选项中，开启显示触摸操作和指针位置，在出现异常的情况下，屏幕上可以正常显示触摸的点。
2，在目前提供的log（异常并打开prop文件夹）中，发现如下现象：
//对应此次down事件，x=349.514557, y=600.530823输入按键2的区域
07-20 15:55:09.973196 12224 12490 D InputReader: AP_PROF:AppLaunch_dispatchPtr:Down:4085464, ID:0, Index:-2095151168
07-20 15:55:09.974684 12224 12490 D InputDispatcher:   Pointer 0: id=0, toolType=1, x=349.514557, y=600.530823, pressure=1.000000, size=0.130000, touchMajor=25.971821, touchMinor=25.971821, toolMajor=25.971821, toolMinor=25.971821, orientation=0.000000
//发现在InputDispatcher会有对应的xOffset=-240.000000, yOffset=-595.000000情况。
07-20 15:55:09.988402 12224 12489 D InputDispatcher: channel '8b6e878  (server)' ~ prepareDispatchCycle - flags=0x00000101, xOffset=-240.000000, yOffset=-595.000000, scaleFactor=1.000000, pointerIds=0x0
07-20 15:55:09.988901 12224 12489 D InputTransport: channel '8b6e878  (server)' publisher ~ publishMotionEvent: seq=1048, deviceId=1, source=0x1002, action=0x0, actionButton=0x00000000, flags=0x0, edgeFlags=0x0, metaState=0x0, buttonState=0x0, xOffset=-240.000000, yOffset=-595.000000, xPrecision=1.001389, yPrecision=1.000781, downTime=4085464482000, eventTime=4085464482000, pointerCount=1
//处理的对应的ACTION_DOWN对应的位置为x[0]=109.51456, y[0]=5.5308228
07-20 15:55:09.998014 13998 13998 V ViewRootImpl: enqueueInputEvent: event = MotionEvent { action=ACTION_DOWN, actionButton=0, id[0]=0, x[0]=109.51456, y[0]=5.5308228, toolType[0]=TOOL_TYPE_FINGER, buttonState=0, metaState=0, flags=0x0, edgeFlags=0x0, pointerCount=1, historySize=0, eventTime=4085464, downTime=4085464, deviceId=1, source=0x1002 },processImmediately = true,mProcessInputEventsScheduled = false, this = ViewRoot{b11b31 ,ident = 0}

想咨询贵司的是，
frameworks\native\services\inputflinger\InputDispatcher.cpp中，
void InputDispatcher::prepareDispatchCycleLocked(nsecs_t currentTime,
        const sp<Connection>& connection, EventEntry* eventEntry, const InputTarget* inputTarget) {
#if DEBUG_DISPATCH_CYCLE
    ALOGD("channel '%s' ~ prepareDispatchCycle - flags=0x%08x, "
            "xOffset=%f, yOffset=%f, scaleFactor=%f, "
            "pointerIds=0x%x",
            connection->getInputChannelName(), inputTarget->flags,
            inputTarget->xOffset, inputTarget->yOffset,
            inputTarget->scaleFactor, inputTarget->pointerIds.value);
#endif

    // Skip this event if the connection status is not normal.
    // We don't want to enqueue additional outbound events if the connection is broken.
    if (connection->status != Connection::STATUS_NORMAL) {
#if DEBUG_DISPATCH_CYCLE
        ALOGD("channel '%s' ~ Dropping event because the channel status is %s",
                connection->getInputChannelName(), connection->getStatusLabel());
#endif
        return;
    }
会有对应的inputTarget->xOffset, inputTarget->yOffset,处理，这块偏移量是由哪块引起的，此块分析是不是造成按键2不能响应的原因，由于项目紧急，还请贵司帮忙分析处理一下！
	
