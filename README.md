## *Implementasi_Prototype_PeduliLindungi*

Berikut ini adalah cara mengimplementasi prototype yang telah dibuat kedalam mobole apps, kami menggunakan tools 
android studio untuk coding dan untuk bahasa pemrograman yang kami gunakan adalah kotlin. design kami adaptasi dari
prototype yang telah dibuat pada tools figma.

adapun langkah langkahnya sebagai berikut

1. Menambahkan refrensi dependencies pada module gradle app

```bash
    //viewPageII
    implementation 'androidx.viewpager2:viewpager2:1.0.0'
    //RoundedImageView
    implementation 'com.makeramen:roundedimageview:2.3.0'
```

2. langkah selanjutnya dalah membuat class baru dengan nama SliderItem.kt dan SliderAdapter.kt, digunakan untuk image slide

**isi class SliderItem.kt**
```bash
package com.example.pedulilindungi

class SliderItem internal constructor(
    val image: Int
)
```

**isi class SliderAdapter.kt**
```bash
package com.example.pedulilindungi

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.RemoteViews
import androidx.recyclerview.widget.RecyclerView
import androidx.viewpager2.widget.ViewPager2
import com.makeramen.roundedimageview.RoundedImageView

class SliderAdapter internal constructor(
    sliderItems: MutableList<SliderItem>,
    viewPager: ViewPager2
): RecyclerView.Adapter<SliderAdapter.SliderViewHolder>(){

    private val sliderItems: List<SliderItem>
    init {
        this.sliderItems = sliderItems
    }

    class SliderViewHolder(itemView: View) :RecyclerView.ViewHolder(itemView){
        private val imageView: RoundedImageView = itemView.findViewById(R.id.imageSlide)

        fun image(sliderItem: SliderItem){
            imageView.setImageResource(sliderItem.image)
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): SliderViewHolder {
        return SliderViewHolder(
            LayoutInflater.from(parent.context).inflate(
                R.layout.slide_item_container,
                parent,
                false
            )
        )
    }

    override fun onBindViewHolder(holder: SliderViewHolder, position: Int) {
        holder.image(sliderItems[position])
    }

    override fun getItemCount(): Int {
        return sliderItems.size
    }
}
```

3. pada mainactivity isikan code seperti dibawah ini, di file ini digunakan untuk pengaturan image slide dan importnya
selanjutnya juga digunakan untuk perpindahan laman / Intent 

**code file MainActivity.kt**

```bash
package com.example.pedulilindungi

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Window
import android.view.WindowManager
import android.widget.TextView
import androidx.core.content.ContextCompat
import androidx.recyclerview.widget.RecyclerView
import androidx.viewpager2.widget.CompositePageTransformer
import androidx.viewpager2.widget.MarginPageTransformer
import androidx.viewpager2.widget.ViewPager2
import java.lang.Math.abs

class MainActivity : AppCompatActivity() {

    private lateinit var viewPager2: ViewPager2

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val window: Window = this@MainActivity.window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS)
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)
        window.statusBarColor = ContextCompat.getColor(this@MainActivity, R.color.cloud1)

        viewPager2 = findViewById(R.id.viewPager_ImageSlider)
        val sliderItems: MutableList<SliderItem> = ArrayList()
        sliderItems.add(SliderItem(R.drawable.sertifikat1))
        sliderItems.add(SliderItem(R.drawable.sertifikat2))

        viewPager2.adapter = SliderAdapter(sliderItems, viewPager2)

        viewPager2.clipToPadding = false
        viewPager2.clipChildren = false
        viewPager2.offscreenPageLimit = 2
        viewPager2.getChildAt(0).overScrollMode = RecyclerView.OVER_SCROLL_NEVER

        val compositePageTransformer = CompositePageTransformer()
        compositePageTransformer.addTransformer(MarginPageTransformer(20))
        compositePageTransformer.addTransformer { page, position ->
            val r  = - abs(position)
            page.scaleY = 0.85f + r * 0.50f
        }
        viewPager2.setPageTransformer(compositePageTransformer)

        val textprof = findViewById<TextView>(R.id.goProfil)
        textprof.setOnClickListener{
            intent = Intent(this, ProfilActivity::class.java)
            startActivity(intent)
        }

        val textpcr = findViewById<TextView>(R.id.goPcr)
        textpcr.setOnClickListener {
            intent = Intent(this, PcrActivity::class.java)
            startActivity(intent)
        }
    }
}
```

4. berikutnya dalah mendesin interface tampilan aplikasi pada file xml

**code xml untuk laman homescreen**
```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/cloud1"
    tools:context=".MainActivity">


    <ImageView
        android:id="@+id/profil"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginStart="24dp"
        android:layout_marginTop="36dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/user" />

    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginTop="36dp"
        android:layout_marginEnd="24dp"
        android:background="@drawable/shape"
        android:padding="11dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/option" />

    <ImageView
        android:id="@+id/imageView4"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginTop="36dp"
        android:layout_marginEnd="6dp"
        android:background="@drawable/shape"
        android:padding="15dp"
        app:layout_constraintEnd_toStartOf="@+id/imageView3"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/bell" />

    <TextView
        android:id="@+id/goProfil"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="52dp"
        android:text="Hi, Azrael"
        android:textColor="@color/white"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toStartOf="@+id/imageView4"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toEndOf="@+id/profil"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Sertifikat Vaksin"
        android:textColor="@color/white"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.07"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/profil" />

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager_ImageSlider"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:paddingStart="70dp"
        android:paddingEnd="70dp"
        app:layout_constraintTop_toBottomOf="@+id/textView2"
        tools:layout_editor_absoluteX="-16dp" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hasil Tes Covid-19"
        android:textColor="@color/white"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.092"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/viewPager_ImageSlider" />

    <TextView
        android:id="@+id/goPcr"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="20dp"
        android:background="@drawable/shapefill"
        android:drawableRight="@drawable/ic_baseline_check_circle_24"
        android:paddingLeft="20dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="35dp"
        android:text="PCR NEGATIF"
        android:textColor="@color/green"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView3" />

    <TextView
        android:id="@+id/textView6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="60dp"
        android:text="Berlaku Hingga 04 Nov 12:00AM"
        android:textColor="@color/cloud3"
        android:textSize="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.192"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView3" />

    <TextView
        android:id="@+id/textView8"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:background="@drawable/shapefill"
        android:paddingLeft="30dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="350dp"
        android:text="Riwayat Chek-In"
        android:textColor="@color/black"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/goPcr" />

    <TextView
        android:id="@+id/textView9"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="Riwayat Lengkap"
        android:textSize="12dp"
        android:drawablePadding="10dp"
        android:textColor="@color/cloud4"
        android:drawableRight="@drawable/ic_baseline_arrow_forward_ios_24"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.916"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textView8" />

    <TextView
        android:id="@+id/textView10"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="68dp"
        android:text="MRT Senayan"
        android:textColor="@color/cloud3"
        android:textSize="18dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.102"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textView8" />

    <TextView
        android:id="@+id/textView11"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:text="Masuk Pukul 01:00PM"
        android:textColor="@color/cloud4"
        android:textSize="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.106"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView10" />

    <TextView
        android:id="@+id/textView12"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="Grand Indonesia"
        android:textColor="@color/cloud3"
        android:textSize="18dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.111"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView11" />

    <TextView
        android:id="@+id/textView13"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:text="10:00AM - 10:30AM"
        android:textColor="@color/cloud4"
        android:textSize="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.101"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView12" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="FX Sudirman"
        android:textColor="@color/cloud3"
        android:textSize="18dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.102"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView13" />

    <TextView
        android:id="@+id/textView7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:text="09:00AM - 09:30AM"
        android:textColor="@color/cloud4"
        android:textSize="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.101"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView5" />

    <TextView
        android:id="@+id/textView14"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="MRT Sudirman"
        android:textColor="@color/cloud3"
        android:textSize="18dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.102"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView7" />

    <TextView
        android:id="@+id/textView15"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:text="09:00AM - 09:30AM"
        android:textColor="@color/cloud4"
        android:textSize="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.097"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView14" />

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="0dp"
        android:layout_height="5dp"
        android:layout_marginStart="28dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="28dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView11"
        app:srcCompat="@drawable/lin" />

    <ImageView
        android:id="@+id/imageView5"
        android:layout_width="0dp"
        android:layout_height="5dp"
        android:layout_marginStart="28dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="28dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView13"
        app:srcCompat="@drawable/lin" />

    <ImageView
        android:id="@+id/imageView6"
        android:layout_width="0dp"
        android:layout_height="5dp"
        android:layout_marginStart="28dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="28dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView7"
        app:srcCompat="@drawable/lin" />

    <ImageView
        android:id="@+id/imageView8"
        android:layout_width="0dp"
        android:layout_height="300dp"
        android:layout_marginTop="612dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/trans" />

    <TextView
        android:id="@+id/textView16"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="110dp"
        android:background="@drawable/shape_barcode"
        android:drawableTop="@drawable/ic_baseline_qr_code_scanner_24"
        android:paddingLeft="30dp"
        android:paddingTop="10dp"
        android:paddingRight="30dp"
        android:paddingBottom="10dp"
        android:text="SCAN QR CODE"
        android:textColor="@color/white"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="@+id/textView8"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
```
**code xml untuk laman profil**
```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/cloud1"
    tools:context=".ProfilActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="415dp"
        android:layout_height="629dp"
        android:layout_marginBottom="264dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.75"
        app:srcCompat="@drawable/background" />

    <ImageView
        android:id="@+id/imageView10"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginBottom="420dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="@+id/imageView"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:srcCompat="@drawable/framepp" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="450dp"
        android:text="Azrael Mustafa"
        android:textColor="@color/white"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.843"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/textView21"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="12dp"
        android:drawableLeft="@drawable/ic_baseline_language_24"
        android:drawablePadding="10dp"
        android:text="Indonesia"
        android:textColor="@color/white"
        android:textSize="17dp"
        app:layout_constraintEnd_toEndOf="@+id/textView4"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView4"
        app:layout_constraintTop_toBottomOf="@+id/textView4" />

    <TextView
        android:id="@+id/textView22"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:drawableLeft="@drawable/ic_outline_email_24"
        android:drawablePadding="10dp"
        android:text="azraelmstf@yahoo.com"
        android:textColor="@color/white"
        android:textSize="17dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.223"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageView10" />

    <TextView
        android:id="@+id/textView23"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:drawableLeft="@drawable/ic_baseline_phone_24"
        android:drawablePadding="10dp"
        android:text="085775245005"
        android:textColor="@color/white"
        android:textSize="17dp"
        app:layout_constraintEnd_toEndOf="@+id/textView22"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView22"
        app:layout_constraintTop_toBottomOf="@+id/textView22" />

    <TextView
        android:id="@+id/textView36"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:text="LogOut"
        android:drawableLeft="@drawable/ic_logout"
        android:drawablePadding="10dp"
        android:textColor="@color/white"
        android:textSize="17dp"
        app:layout_constraintEnd_toEndOf="@+id/textView23"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView23"
        app:layout_constraintTop_toBottomOf="@+id/textView23" />

    <TextView
        android:id="@+id/textToSert1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:background="@drawable/shapefill"
        android:drawableLeft="@drawable/ic_vaccines"
        android:drawableRight="@drawable/ic_baseline_arrow_forward_ios_24"
        android:drawablePadding="10dp"
        android:paddingLeft="20dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="35dp"
        android:text="Histori Vaksinasi Pertama"
        android:textColor="@color/black"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView36" />

    <TextView
        android:id="@+id/textView38"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="44dp"
        android:text="Sertivikat Vaksin"
        android:textColor="@color/cloud3"
        android:textSize="14dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.229"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textToSert1" />

    <TextView
        android:id="@+id/textToSert2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:background="@drawable/shapefill"
        android:drawableLeft="@drawable/ic_vaccines"
        android:drawableRight="@drawable/ic_baseline_arrow_forward_ios_24"
        android:drawablePadding="10dp"
        android:paddingLeft="20dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="35dp"
        android:text="Histori Vaksinasi Kedua"
        android:textColor="@color/black"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="@+id/textToSert1"
        app:layout_constraintStart_toStartOf="@+id/textToSert1"
        app:layout_constraintTop_toBottomOf="@+id/textToSert1" />

    <TextView
        android:id="@+id/textView40"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="44dp"
        android:text="Sertivikat Vaksin"
        android:textColor="@color/cloud3"
        android:textSize="14dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.229"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textToSert2" />

    <TextView
        android:id="@+id/textBackProf"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:drawableLeft="@drawable/ic_baseline_arrow_back_24"
        android:text=""
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.054"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

**code xml untuk laman hasil tes pcr**
```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/cloud1"
    tools:context=".PcrActivity">

    <ImageView
        android:id="@+id/profil"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginStart="24dp"
        android:layout_marginTop="36dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/user" />

    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginTop="36dp"
        android:layout_marginEnd="24dp"
        android:background="@drawable/shape"
        android:padding="11dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/option" />

    <ImageView
        android:id="@+id/imageView4"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginTop="36dp"
        android:layout_marginEnd="6dp"
        android:background="@drawable/shape"
        android:padding="15dp"
        app:layout_constraintEnd_toStartOf="@+id/imageView3"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/bell" />

    <TextView
        android:id="@+id/goProfil"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="52dp"
        android:fontFamily="@font/basic"
        android:text="Hi, Azrael"
        android:textSize="18dp"
        android:textColor="@color/white"
        android:textStyle="bold"
        app:layout_constraintEnd_toStartOf="@+id/imageView4"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toEndOf="@+id/profil"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView17"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="150dp"
        android:background="@drawable/shapefill"
        android:paddingLeft="140dp"
        android:paddingTop="70dp"
        android:paddingRight="20dp"
        android:paddingBottom="620dp"
        android:text="HASIL TES PCR"
        android:textColor="@color/green"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView18"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="100dp"
        android:text="NEGATIF"
        android:textColor="@color/green"
        android:textSize="18dp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textView17" />

    <TextView
        android:id="@+id/textView19"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:text="Nama                     :"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.064"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView18" />

    <TextView
        android:id="@+id/textView20"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:text="Azrael Mustafa"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.263"
        app:layout_constraintStart_toEndOf="@+id/textView19"
        app:layout_constraintTop_toBottomOf="@+id/textView18" />

    <TextView
        android:id="@+id/textView24"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Umur                      :"
        app:layout_constraintEnd_toEndOf="@+id/textView19"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView19"
        app:layout_constraintTop_toBottomOf="@+id/textView19" />

    <TextView
        android:id="@+id/textView25"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="21 Tahun"
        app:layout_constraintEnd_toEndOf="@+id/textView20"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView20"
        app:layout_constraintTop_toBottomOf="@+id/textView20" />

    <TextView
        android:id="@+id/textView26"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Jenis Kelamin       :"
        app:layout_constraintEnd_toEndOf="@+id/textView24"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView24"
        app:layout_constraintTop_toBottomOf="@+id/textView24" />

    <TextView
        android:id="@+id/textView27"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Laki Laki"
        app:layout_constraintEnd_toEndOf="@+id/textView25"
        app:layout_constraintStart_toStartOf="@+id/textView25"
        app:layout_constraintTop_toBottomOf="@+id/textView25" />

    <TextView
        android:id="@+id/textView28"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Alamat                   :"
        app:layout_constraintEnd_toEndOf="@+id/textView26"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView26"
        app:layout_constraintTop_toBottomOf="@+id/textView26" />

    <TextView
        android:id="@+id/textView29"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Kota Yogyakarta, DIY 55142"
        app:layout_constraintEnd_toEndOf="@+id/textView27"
        app:layout_constraintHorizontal_bias="0.016"
        app:layout_constraintStart_toStartOf="@+id/textView27"
        app:layout_constraintTop_toBottomOf="@+id/textView27" />

    <TextView
        android:id="@+id/textView30"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Tgl Registrasi        :"
        app:layout_constraintEnd_toEndOf="@+id/textView28"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView28"
        app:layout_constraintTop_toBottomOf="@+id/textView28" />

    <TextView
        android:id="@+id/textView31"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="25 Oktober 2021"
        app:layout_constraintEnd_toEndOf="@+id/textView29"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView29"
        app:layout_constraintTop_toBottomOf="@+id/textView29" />

    <TextView
        android:id="@+id/textView32"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Tgl Periksa            :"
        app:layout_constraintEnd_toEndOf="@+id/textView30"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView30"
        app:layout_constraintTop_toBottomOf="@+id/textView30" />

    <TextView
        android:id="@+id/textView33"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="25 Oktober 2021"
        app:layout_constraintEnd_toEndOf="@+id/textView31"
        app:layout_constraintStart_toStartOf="@+id/textView31"
        app:layout_constraintTop_toBottomOf="@+id/textView31" />

    <TextView
        android:id="@+id/textView34"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Instansi                 :"
        app:layout_constraintEnd_toEndOf="@+id/textView32"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView32"
        app:layout_constraintTop_toBottomOf="@+id/textView32" />

    <TextView
        android:id="@+id/textView35"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:text="Klinik Cito Yogyakarta"
        app:layout_constraintEnd_toEndOf="@+id/textView33"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/textView33"
        app:layout_constraintTop_toBottomOf="@+id/textView33" />

    <TextView
        android:id="@+id/textBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text=""
        android:drawableTop="@drawable/ic_baseline_keyboard_arrow_down_24"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/textView17" />

    <ImageView
        android:id="@+id/imageView7"
        android:layout_width="350dp"
        android:layout_height="127dp"
        android:layout_marginBottom="100dp"
        app:layout_constraintBottom_toBottomOf="@+id/textView17"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:srcCompat="@drawable/hasil" />


</androidx.constraintlayout.widget.ConstraintLayout>
```

**code xml untuk laman unduh sertifikat1**
```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/cloud1"
    tools:context=".Sertifikat1Activity">

    <ImageView
        android:id="@+id/imageView11"
        android:layout_width="366dp"
        android:layout_height="256dp"
        android:layout_marginTop="20dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.592"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView37"
        app:srcCompat="@drawable/sertifikat1" />

    <TextView
        android:id="@+id/textView37"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="250dp"
        android:text="Sertifikat Vaksin Dosis Pertama"
        android:textSize="18dp"
        android:textStyle="bold"
        android:textColor="@color/white"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView39"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="50dp"
        android:background="@drawable/shapefill"
        android:drawableLeft="@drawable/ic_download"
        android:drawablePadding="10dp"
        android:paddingLeft="20dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="20dp"
        android:text="Download Sertifikat"
        android:textColor="@color/cloud3"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.301"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/gotBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="50dp"
        android:drawableLeft="@drawable/ic_close"
        android:text=""
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.373"
        app:layout_constraintStart_toEndOf="@+id/textView39" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

**code xml untuk laman unduh sertifikat2**
```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/cloud1"
    tools:context=".Sertifikat2Activity">

    <ImageView
        android:id="@+id/imageView11"
        android:layout_width="366dp"
        android:layout_height="256dp"
        android:layout_marginTop="16dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView37"
        app:srcCompat="@drawable/sertifikat2" />

    <TextView
        android:id="@+id/textView37"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="250dp"
        android:text="Sertifikat Vaksin Dosis Kedua"
        android:textSize="18dp"
        android:textStyle="bold"
        android:textColor="@color/white"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView39"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="50dp"
        android:background="@drawable/shapefill"
        android:drawableLeft="@drawable/ic_download"
        android:drawablePadding="10dp"
        android:paddingLeft="20dp"
        android:paddingTop="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="20dp"
        android:text="Download Sertifikat"
        android:textColor="@color/cloud3"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.301"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/gotBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="50dp"
        android:drawableLeft="@drawable/ic_close"
        android:text=""
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.373"
        app:layout_constraintStart_toEndOf="@+id/textView39" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

5. pada tahap selanjutanya adalah fungsionalitas perpindahan laman atau Intent explicit dan karena main telah ditampilkan di atas maka selanjutnya adalah file .kt lainya

**code file ProfilActivity.kt**

```bash
package com.example.pedulilindungi

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Window
import android.view.WindowManager
import android.widget.TextView
import androidx.core.content.ContextCompat

class ProfilActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_profil)

        val window: Window = this@ProfilActivity.window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS)
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)
        window.statusBarColor = ContextCompat.getColor(this@ProfilActivity, R.color.cloud3)

        val backarrow = findViewById<TextView>(R.id.textBackProf)
        backarrow.setOnClickListener {
            intent = Intent(this, MainActivity::class.java)
            startActivity(intent)
        }
        val gosertifikat1 = findViewById<TextView>(R.id.textToSert1)
        gosertifikat1.setOnClickListener {
            intent = Intent(this, Sertifikat1Activity::class.java)
            startActivity(intent)
        }
        val gosertifikat2 = findViewById<TextView>(R.id.textToSert2)
        gosertifikat2.setOnClickListener {
            intent = Intent(this, Sertifikat2Activity::class.java)
            startActivity(intent)
        }
    }
}
```

**code file PcrActivity.kt**

```bash
package com.example.pedulilindungi

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Window
import android.view.WindowManager
import android.widget.TextView
import androidx.core.content.ContextCompat

class PcrActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_pcr)

        val window: Window = this@PcrActivity.window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS)
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)
        window.statusBarColor = ContextCompat.getColor(this@PcrActivity, R.color.cloud1)

        val backarrow = findViewById<TextView>(R.id.textBack)
        backarrow.setOnClickListener {
            intent = Intent(this, MainActivity::class.java)
            startActivity(intent)
        }
    }
}
```

**code file Sertifikat1Activity.kt**

```bash
package com.example.pedulilindungi

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Window
import android.view.WindowManager
import android.widget.TextView
import androidx.core.content.ContextCompat

class Sertifikat1Activity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_sertifikat1)

        val window: Window = this@Sertifikat1Activity.window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS)
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)
        window.statusBarColor = ContextCompat.getColor(this@Sertifikat1Activity, R.color.cloud1)

        val close = findViewById<TextView>(R.id.gotBack)
        close.setOnClickListener {
            intent = Intent(this, ProfilActivity::class.java)
            startActivity(intent)
        }
    }
}
```

**code file Sertifikat2Activity.kt**

```bash
package com.example.pedulilindungi

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Window
import android.view.WindowManager
import android.widget.TextView
import androidx.core.content.ContextCompat

class Sertifikat2Activity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_sertifikat2)

        val window: Window = this@Sertifikat2Activity.window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS)
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)
        window.statusBarColor = ContextCompat.getColor(this@Sertifikat2Activity, R.color.cloud1)

        val close = findViewById<TextView>(R.id.gotBack)
        close.setOnClickListener {
            intent = Intent(this, ProfilActivity::class.java)
            startActivity(intent)
        }
    }
}
```



