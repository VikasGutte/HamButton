# HamButton

![Ham Button](Ham.gif "Ham Button"=250x100)

Gradle
implementation 'com.nightonke:boommenu:1.0.9'

Main Activity

    public class MainActivity extends AppCompatActivity {

      private boolean init = false;
      private BoomMenuButton boomMenuButton;

      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          boomMenuButton = findViewById(R.id.boom);

      }

      @Override
      public void onWindowFocusChanged(boolean hasFocus) {
          super.onWindowFocusChanged(hasFocus);

          // Use a param to record whether the boom button has been initialized
          // Because we don't need to init it again when onResume()
          if (init) return;
          init = true;

          Drawable[] subButtonDrawables = new Drawable[3];
          int[] drawablesResource = new int[]{
                  R.drawable.search,
                  R.drawable.refresh,
                  R.drawable.settings
          };
          for (int i = 0; i < 3; i++)
              subButtonDrawables[i] = ContextCompat.getDrawable(this, drawablesResource[i]);

          int[][] subButtonColors = new int[3][2];
          for (int i = 0; i < 3; i++) {
              subButtonColors[i][1] = ContextCompat.getColor(this, R.color.textcolor);
              subButtonColors[i][0] = Util.getInstance().getPressedColor(subButtonColors[i][1]);
          }

          // Now with Builder, you can init BMB more convenient
          new BoomMenuButton.Builder()
                  .addSubButton(ContextCompat.getDrawable(this, R.drawable.search), subButtonColors[0], "Search")
                  .addSubButton(ContextCompat.getDrawable(this, R.drawable.refresh), subButtonColors[0], "Refresh")
                  .addSubButton(ContextCompat.getDrawable(this, R.drawable.settings), subButtonColors[0], "Setting")
                  .button(ButtonType.HAM)
                  .boom(BoomType.PARABOLA)
                  .place(PlaceType.HAM_3_1)
                  .subButtonTextColor(ContextCompat.getColor(this, R.color.colorPrimaryDark))
                  .subButtonsShadow(Util.getInstance().dp2px(2), Util.getInstance().dp2px(2))
                  .onSubButtonClick(new BoomMenuButton.OnSubButtonClickListener() {
                      @Override
                      public void onClick(int buttonIndex) {
                          switch (buttonIndex) {
                              default:
                                  Toast.makeText(getApplicationContext(), "Ham Button Clicked", Toast.LENGTH_SHORT).show();
                                  break;
                          }
                      }
                  })
                  .init(boomMenuButton);
      }
    }
