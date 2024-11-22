# XHAW-WIL5112
App Code 
@file:OptIn(ExperimentalMaterial3Api::class)

package com.example.empoweringthenation1

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.width
import androidx.compose.foundation.layout.wrapContentSize
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.Button
import androidx.compose.material3.Card
import androidx.compose.material3.CardDefaults
import androidx.compose.material3.Checkbox
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.SmallTopAppBar
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.navigation.NavHostController
import androidx.navigation.NavType
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController
import androidx.navigation.navArgument
import com.example.empoweringthenation1.ui.theme.EmpoweringTheNation1Theme

@Parcelize
data class Course(
    val name: String,
    val description: String,
    val fee: Int
) {

}

val sampleCourses = listOf(
    Course("First Aid", "Purpose: To provide first aid awareness and basic life support.\n" +
            "\n" +
            "Course Content\n" +
            "Wounds and bleeding\n" +
            "Burns and fractures\n" +
            "Emergency scene management\n" +
            "Cardio-Pulmonary Resuscitation (CPR)\n" +
            "Respiratory distress (e.g., choking, blocked airway)", 1500),
    Course("Sewing",
            "\n" +
            "Purpose: To provide alterations and new garment tailoring services.\n" +
            "\n" +
            "Course Content\n" +
            "Types of stitches\n" +
            "Threading a sewing machine\n" +
            "Sewing buttons, zips, hems, and seams\n" +
            "Alterations\n" +
            "Designing and sewing new garments", 1500),
    Course("Landscaping", "Purpose: To provide landscaping services for new and established gardens.\n" +
            "\n" +
            "Course Content\n" +
            "Indigenous and exotic plants and trees\n" +
            "Fixed structures (fountains, statues, benches, tables, built-in braai)\n" +
            "Balancing plants and trees in a garden\n" +
            "Aesthetics of plant shapes and colours\n" +
            "Garden layout", 1500),
    Course("Life Skills", "Purpose: To provide skills to navigate basic life necessities.\n" +
            "\n" +
            "Course Content\n" +
            "Opening a bank account\n" +
            "Basic labour law (know your rights)\n" +
            "Basic reading and writing literacy\n" +
            "Basic numeric literacy", 1500),
    Course("Child Minding", "Purpose: To provide basic child and baby care.\n" +
            "\n" +
            "Course Content\n" +
            "Birth to six-month-old baby needs\n" +
            "Seven-month to one-year-old needs\n" +
            "Toddler needs\n" +
            "Educational toys\n", 750),
    Course("Cooking", "Purpose: To prepare and cook nutritious family meals.\n" +
            "\n" +
            "Course Content\n" +
            "Nutritional requirements for a healthy body\n" +
            "Types of protein, carbohydrates, and vegetables\n" +
            "Planning meals\n" +
            "Preparation and cooking of meals\n", 750),
    Course("Garden Maintenance", "Purpose: To provide basic knowledge of watering, pruning, and planting in a domestic garden.\n" +
            "\n" +
            "Course Content\n" +
            "Water restrictions and watering requirements of indigenous and exotic plants\n" +
            "Pruning and propagation of plants\n" +
            "Planting techniques for different plant types\n", 750)
)

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            EmpoweringTheNation1Theme {
                NavGraph()
            }
        }
    }
}

@Composable
fun NavGraph(startDestination: String = "main") {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = startDestination) {
        composable("main") { MainScreen(navController) }
        composable("courseList") { CourseListScreen(navController, sampleCourses) }
        composable(
            route = "courseDetails/{courseName}",
            arguments = listOf(navArgument("courseName") { type = NavType.StringType })
        ) { backStackEntry ->
            val courseName = backStackEntry.arguments?.getString("courseName")
            val course = sampleCourses.find { it.name == courseName }
            if (course != null) {
                CourseDetailsScreen(navController, course)
            } else {
                Text("Course not found")
            }
        }
        composable("calculateFees") { CalculateFeesScreen(navController, sampleCourses) }
        composable("contact") { ContactScreen(navController) }
        composable("info") { InfoScreen(navController) }
        composable("about") { AboutScreen(navController) }
        composable("service") { ServiceScreen(navController) }
        composable("splash") { SplashScreen(navController) }
    }
}

@Composable
fun SplashScreen(navController: NavHostController) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .wrapContentSize(Alignment.Center)
    ) {
        Text(text = "Splash Screen", style = MaterialTheme.typography.displayMedium)
        Button(onClick = { navController.navigate("main") }) {
            Text("Go to Home")
        }
    }
}

@Composable
fun MainScreen(navController: NavHostController) {
    val backgroundImage = painterResource(id = R.drawable.nnn) // Replace with your image resource

    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("Tech Wizards") }
            )
        }
    ) { innerPadding ->
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.Black) // Fallback color in case image is not loaded
        ) {
            // Background Image
            Image(
                painter = backgroundImage,
                contentDescription = null,
                contentScale = ContentScale.Crop, // Adjust scale as needed
                modifier = Modifier
                    .fillMaxSize()
            )

            // Overlay Content
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(innerPadding)
                    .padding(16.dp),
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.Center
            ) {
                Text(
                    text = "Welcome to Empowering the Nation",
                    style = MaterialTheme.typography.displayMedium,
                    modifier = Modifier.align(Alignment.CenterHorizontally)
                )
                Spacer(modifier = Modifier.height(16.dp))
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Button(
                        onClick = { navController.navigate("courseList") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("View Courses")
                    }
                    Spacer(modifier = Modifier.width(16.dp))
                    Button(
                        onClick = { navController.navigate("calculateFees") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("Calculate Fees")
                    }
                }
                Spacer(modifier = Modifier.height(16.dp))
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Button(
                        onClick = { navController.navigate("contact") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("Contact")
                    }
                    Spacer(modifier = Modifier.width(16.dp))
                    Button(
                        onClick = { navController.navigate("info") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("Info")
                    }
                }
                Spacer(modifier = Modifier.height(16.dp))
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Button(
                        onClick = { navController.navigate("about") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("About")
                    }
                    Spacer(modifier = Modifier.width(16.dp))
                    Button(
                        onClick = { navController.navigate("service") },
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("Service")
                    }
                }
            }
        }
    }
}

@Composable
fun CourseListScreen(navController: NavHostController, courses: List<Course>) {
    LazyColumn(modifier = Modifier.padding(16.dp)) {
        items(courses) { course ->
            CourseItem(course) {
                navController.navigate("courseDetails/${course.name}")
            }
        }
    }
}

@Composable
fun CourseItem(course: Course, onCourseClick: (Course) -> Unit) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 8.dp)
            .clickable { onCourseClick(course) },
        elevation = CardDefaults.cardElevation(4.dp)
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(course.name, style = MaterialTheme.typography.displayMedium)
            Spacer(modifier = Modifier.height(4.dp))
            Text(course.description)
            Spacer(modifier = Modifier.height(4.dp))
            Text("Fee: R${course.fee}")
        }
    }
}

@Composable
fun CourseDetailsScreen(navController: NavHostController, course: Course) {
    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text(course.name) },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp)
        ) {
            Text(
                text = course.name,
                style = MaterialTheme.typography.displaySmall,
                modifier = Modifier.align(Alignment.CenterHorizontally)
            )
            Spacer(modifier = Modifier.height(8.dp))
            Text(
                text = course.description,
                modifier = Modifier.align(Alignment.Start)
            )
            Spacer(modifier = Modifier.height(8.dp))
            Text(
                text = "Fee: R${course.fee}",
                modifier = Modifier.align(Alignment.Start)
            )
            Spacer(modifier = Modifier.height(16.dp)) // Add some space before the button
            Button(
                onClick = { navController.navigate("main") },
                modifier = Modifier.align(Alignment.CenterHorizontally)
            ) {
                Text("Back to Home")
            }
        }
    }
}

@Composable
fun CalculateFeesScreen(navController: NavHostController, courses: List<Course>) {
    var selectedCourses by remember { mutableStateOf(listOf<Course>()) }
    var totalFee by remember { mutableStateOf(0) }

    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("Calculate Fees") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp)
        ) {
            LazyColumn(modifier = Modifier.weight(1f)) {
                items(courses) { course ->
                    Row(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(vertical = 8.dp)
                            .clickable {
                                if (selectedCourses.contains(course)) {
                                    selectedCourses = selectedCourses - course
                                } else {
                                    selectedCourses = selectedCourses + course
                                }
                                totalFee = selectedCourses.sumOf { it.fee }
                            },
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        Checkbox(
                            checked = selectedCourses.contains(course),
                            onCheckedChange = {
                                if (it) {
                                    selectedCourses = selectedCourses + course
                                } else {
                                    selectedCourses = selectedCourses - course
                                }
                                totalFee = selectedCourses.sumOf { it.fee }
                            }
                        )
                        Text(
                            text = course.name,
                            modifier = Modifier.padding(start = 8.dp)
                        )
                    }
                }
            }
            Spacer(modifier = Modifier.height(16.dp))
            Text("Total Fee: R$totalFee", style = MaterialTheme.typography.displaySmall)
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("main") }) {
                Text("Back to Home")
            }
        }
    }
}

@Composable
fun ContactScreen(navController: NavHostController) {
    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("Contact") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text("Contact Information", style = MaterialTheme.typography.displaySmall)
            Spacer(modifier = Modifier.height(16.dp))
            Text("Email: contact@example.com")
            Spacer(modifier = Modifier.height(8.dp))
            Text("Phone: 123-456-7890")
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("about") }) {
                Text("About")
            }
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("main") }) {
                Text("Back to Home")
            }
        }
    }
}

@Composable
fun InfoScreen(navController: NavHostController) {
    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("Info") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text("Information", style = MaterialTheme.typography.displaySmall)
            Spacer(modifier = Modifier.height(16.dp))
            Text("This is a course management app providing various courses.")
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("service") }) {
                Text("Service")
            }
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("main") }) {
                Text("Back to Home")
            }
        }
    }
}

@Composable
fun AboutScreen(navController: NavHostController) {
    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("About") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text("About Us", style = MaterialTheme.typography.displaySmall)
            Spacer(modifier = Modifier.height(16.dp))
            Text("We are dedicated to providing quality education and skills training.")
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("main") }) {
                Text("Back to Home")
            }
        }
    }
}

@Composable
fun ServiceScreen(navController: NavHostController) {
    Scaffold(
        topBar = {
            SmallTopAppBar(
                title = { Text("Service") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text("Our Services", style = MaterialTheme.typography.displaySmall)
            Spacer(modifier = Modifier.height(16.dp))
            Text("We offer a variety of courses and training programs to help you succeed.")
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { navController.navigate("main") }) {
                Text("Back to Home")
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    EmpoweringTheNation1Theme {
        MainScreen(rememberNavController())
    }
}
