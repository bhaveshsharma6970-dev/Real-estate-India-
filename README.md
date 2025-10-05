# Real-estate-India-
Ismein aap bye sel kar sakte ho property rent ke upar aap le sakte ho sel kar sakte ho aapko yahan per photo dalkar upload hi karna hai aur yah sabse best Indian website hai yahan per aap total hi work kar sakte ho aap apna
/* Complete Java Android Real Estate Rent App (Starter Version) This includes MainActivity, AddPropertyActivity, PropertyDetailActivity, Property model class, and XML layouts. */

// Property.java package com.example.rentapp;

public class Property { private String title; private String description; private String address; private double rent;

public Property() {}

public Property(String title, String description, String address, double rent) {
    this.title = title;
    this.description = description;
    this.address = address;
    this.rent = rent;
}

public String getTitle() { return title; }
public String getDescription() { return description; }
public String getAddress() { return address; }
public double getRent() { return rent; }

public void setTitle(String title) { this.title = title; }
public void setDescription(String description) { this.description = description; }
public void setAddress(String address) { this.address = address; }
public void setRent(double rent) { this.rent = rent; }

}

// MainActivity.java package com.example.rentapp;

import androidx.appcompat.app.AppCompatActivity; import android.content.Intent; import android.os.Bundle; import android.widget.ArrayAdapter; import android.widget.Button; import android.widget.ListView; import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

ArrayList<Property> propertyList;
ArrayList<String> propertyTitles;
ArrayAdapter<String> adapter;
ListView listView;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    listView = findViewById(R.id.propertyListView);
    Button addBtn = findViewById(R.id.addPropertyButton);

    propertyList = new ArrayList<>();
    propertyTitles = new ArrayList<>();

    adapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, propertyTitles);
    listView.setAdapter(adapter);

    addBtn.setOnClickListener(v -> {
        Intent intent = new Intent(MainActivity.this, AddPropertyActivity.class);
        startActivityForResult(intent, 1);
    });

    listView.setOnItemClickListener((parent, view, position, id) -> {
        Property selected = propertyList.get(position);
        Intent intent = new Intent(MainActivity.this, PropertyDetailActivity.class);
        intent.putExtra("title", selected.getTitle());
        intent.putExtra("description", selected.getDescription());
        intent.putExtra("address", selected.getAddress());
        intent.putExtra("rent", selected.getRent());
        startActivity(intent);
    });
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if(requestCode == 1 && resultCode == RESULT_OK){
        String title = data.getStringExtra("title");
        String description = data.getStringExtra("description");
        String address = data.getStringExtra("address");
        double rent = data.getDoubleExtra("rent", 0.0);

        Property property = new Property(title, description, address, rent);
        propertyList.add(property);
        propertyTitles.add(title);
        adapter.notifyDataSetChanged();
    }
}

}

// AddPropertyActivity.java package com.example.rentapp;

import androidx.appcompat.app.AppCompatActivity; import android.content.Intent; import android.os.Bundle; import android.widget.Button; import android.widget.EditText;

public class AddPropertyActivity extends AppCompatActivity {

EditText titleEdit, descEdit, addressEdit, rentEdit;
Button saveBtn;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_add_property);

    titleEdit = findViewById(R.id.titleEdit);
    descEdit = findViewById(R.id.descEdit);
    addressEdit = findViewById(R.id.addressEdit);
    rentEdit = findViewById(R.id.rentEdit);
    saveBtn = findViewById(R.id.saveButton);

    saveBtn.setOnClickListener(v -> {
        String title = titleEdit.getText().toString();
        String desc = descEdit.getText().toString();
        String address = addressEdit.getText().toString();
        double rent = Double.parseDouble(rentEdit.getText().toString());

        Intent resultIntent = new Intent();
        resultIntent.putExtra("title", title);
        resultIntent.putExtra("description", desc);
        resultIntent.putExtra("address", address);
        resultIntent.putExtra("rent", rent);

        setResult(RESULT_OK, resultIntent);
        finish();
    });
}

}

// PropertyDetailActivity.java package

