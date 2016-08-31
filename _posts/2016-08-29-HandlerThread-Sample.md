---
layout: post
category: 技术
tags: [Java, Android, 示例]
title: HandlerThread Sample
header: no
---

- 代码示例

{% highlight java %}
public class HandlerThreadActivity extends AppCompatActivity {

    @BindView(R.id.tvPrompt) TextView mTvPrompt;
    @BindView(R.id.btnStart) Button   mBtnStart;
    @BindView(R.id.btnStop)  Button   mBtnStop;

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.t_act_handler_thread);
        ButterKnife.bind(this);

        HandlerThread handlerThread = new HandlerThread("handlerThread-1");
        handlerThread.start();

        Handler handler = new Handler(handlerThread.getLooper());
        Runnable work = () -> doWork();

        mBtnStart.setOnClickListener((v) -> handler.post(work));
        mBtnStop.setOnClickListener((v) -> {
            if (handlerThread.isAlive())
                handlerThread.interrupt();
        });
    }

    private void doWork() {

        while (true) {

            runOnUiThread(() -> mTvPrompt.append("* "));

            if (Thread.currentThread().isInterrupted())
                break;

            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}
{% endhighlight %}
