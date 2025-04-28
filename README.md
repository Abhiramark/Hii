# Hii
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            BillScreen()
        }
    }
}

@Composable
fun BillScreen() {
    var prevReading by remember { mutableStateOf("") }
    var currentReading by remember { mutableStateOf("") }
    var bill by remember { mutableStateOf("") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        TextField(
            value = prevReading,
            onValueChange = { prevReading = it },
            label = { Text("Enter reading of previous month") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(16.dp))

        TextField(
            value = currentReading,
            onValueChange = { currentReading = it },
            label = { Text("Enter reading of current month") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(16.dp))

        Button(onClick = {
            val prevValue = prevReading.toIntOrNull()
            val currValue = currentReading.toIntOrNull()

            if (prevValue == null || currValue == null || currValue < prevValue) {
                bill = "Please enter valid readings"
            } else {
                val usage = currValue - prevValue
                val amount = if (usage > 50) usage * 5 else usage * 3
                bill = "Your Bill Amount is ₹%.2f".format(amount.toDouble())
            }
        }) {
            Text("Submit and Calculate")
        }

        Spacer(modifier = Modifier.height(16.dp))

        Text(text = bill, style = MaterialTheme.typography.headlineMedium)
    }
}

@Preview(showBackground = true)
@Composable
fun BillScreenPreview() {
    BillScreen()
}
