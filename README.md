# BrainTree-Payment-Android
Check krleyo


apply plugin: 'com.android.application'

android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.a.brain"
        minSdkVersion 21
        targetSdkVersion 27
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])



    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    //noinspection GradleCompatible
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support:design:27.1.1'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
implementation 'com.braintreepayments:card-form:3.1.1'
}




<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">
    <com.braintreepayments.cardform.view.CardForm
        android:layout_height="match_parent"
        android:layout_width="match_parent"
        android:id="@+id/idcard"
        />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="buynow"
        android:id="@+id/btnbuy"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</LinearLayout>




package com.a.brain;

import android.content.DialogInterface;
import android.support.v7.app.AlertDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.InputType;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.braintreepayments.cardform.view.CardForm;

public class MainActivity extends AppCompatActivity {

    CardForm cardForm;
    Button buy;
    AlertDialog.Builder alertbuilder;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    cardForm=findViewById(R.id.idcard);
    buy=findViewById(R.id.btnbuy);
    cardForm.cardRequired(true)
            .expirationRequired(true)
            .cvvRequired(true)
            .postalCodeRequired(true)
            .mobileNumberRequired(true)
            .mobileNumberExplanation("SMS is required on this number")
            .setup(MainActivity.this);
    cardForm.getCvvEditText().setInputType(InputType.TYPE_CLASS_NUMBER|InputType.TYPE_NUMBER_VARIATION_PASSWORD);
   buy.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View v) {
           if (cardForm.isValid()) {
               alertbuilder = new AlertDialog.Builder(MainActivity.this);
               alertbuilder.setTitle("confirm before purchase");
               alertbuilder.setMessage("card number :" + cardForm.getCardNumber() + "\n" +
                       "card expiry date" + cardForm.getExpirationDateEditText().getText().toString() + "\n" +
                       "card cvv : " + cardForm.getCvv() + "\n" +
                       "Postal code :" + cardForm.getPostalCode() + "\n" +
                       "phone number :" + cardForm.getMobileNumber());
               alertbuilder.setPositiveButton("confirm", new DialogInterface.OnClickListener() {
                   @Override
                   public void onClick(DialogInterface dialogInterface, int i) {
                       dialogInterface.dismiss();
                       Toast.makeText(MainActivity.this, "Thank you", Toast.LENGTH_SHORT).show();
                   }
               });
               alertbuilder.setNegativeButton("cancel", new DialogInterface.OnClickListener() {
                   @Override
                   public void onClick(DialogInterface dialogInterface, int which) {
                       dialogInterface.dismiss();
                   }


               });
               AlertDialog alertDialog = alertbuilder.create();
               alertDialog.show();

           } else {
               Toast.makeText(MainActivity.this, "Please complete the form ", Toast.LENGTH_SHORT).show();

           }
       }
   });
    }
}


