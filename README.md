# Rounded-ImageViews-in-Android-Studio
In this repo i have described how you can get that classy rounded ImageView for your Profile Photos using Picasso Library.

//Add the following line to your build.gradle(:app) file at the dependancies level
 implementation 'com.squareup.picasso:picasso:2.8'
 
 // Add the following class file which is also added in the repo but here are the codes
 import android.graphics.Bitmap;
import android.graphics.BitmapShader;
import android.graphics.Canvas;
import android.graphics.Paint;

import com.squareup.picasso.Transformation;


public class RoundedTransformation implements Transformation {
        @Override
        public Bitmap transform(Bitmap source) {
            int size = Math.min(source.getWidth(), source.getHeight());

            int x = (source.getWidth() - size) / 2;
            int y = (source.getHeight() - size) / 2;

            Bitmap squaredBitmap = Bitmap.createBitmap(source, x, y, size, size);
            if (squaredBitmap != source) {
                source.recycle();
            }

            Bitmap bitmap = Bitmap.createBitmap(size, size, source.getConfig());

            Canvas canvas = new Canvas(bitmap);
            Paint paint = new Paint();
            BitmapShader shader = new BitmapShader(squaredBitmap,
                    BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP);
            paint.setShader(shader);
            paint.setAntiAlias(true);

            float r = size / 2f;
            canvas.drawCircle(r, r, r, paint);

            squaredBitmap.recycle();
            return bitmap;
        }

        @Override
        public String key() {
            return "circle";
        }



}

//Next in your main Activity add these codes at the onCreate() method
 Picasso.get()
                .load(R.drawable.your_drawable)
                .transform(new RoundedTransformation() )
                .into(imageView);
                
                
