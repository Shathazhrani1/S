import React, { useState } from 'react';
import { 
  StyleSheet, 
  Text, 
  View, 
  TextInput, 
  TouchableOpacity, 
  Alert, 
  KeyboardAvoidingView, 
  Platform, 
  FlatList, 
  Button, 
  SafeAreaView, 
  Image,
  ScrollView
} from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import Icon from 'react-native-vector-icons/Ionicons';

// Login Screen Component
const LoginScreen = ({ navigation }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = () => {
    if (!email || !password) {
      Alert.alert('Error', 'Please fill in all fields');
      return;
    }
    navigation.navigate('MainApp');
  };

  return (
    <KeyboardAvoidingView
      behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
      style={styles.container}
    >
      <View style={styles.innerContainer}>
        <Text style={styles.logo}>🏥 EMR App</Text>
        
        <View style={styles.inputView}>
          <TextInput
            style={styles.inputText}
            placeholder="National ID..."
            placeholderTextColor="#003f5c"
            onChangeText={setEmail}
            value={email}
            keyboardType="email-address"
            autoCapitalize="none"
          />
        </View>
        
        <View style={styles.inputView}>
          <TextInput
            style={styles.inputText}
            placeholder="Password..."
            placeholderTextColor="#003f5c"
            secureTextEntry
            onChangeText={setPassword}
            value={password}
          />
        </View>

        <TouchableOpacity style={styles.loginBtn} onPress={handleLogin}>
          <Text style={styles.loginText}>LOGIN</Text>
        </TouchableOpacity>

        <View style={styles.signupContainer}>
          <Text style={styles.signupText}>Don't have an account?</Text>
          <TouchableOpacity>
            <Text style={styles.signupLink}> Sign up</Text>
          </TouchableOpacity>
        </View>
      </View>
    </KeyboardAvoidingView>
  );
};

// Home Tab Screen
// Home Tab Screen
const HomeScreen = ({ navigation }) => {
  const upcomingAppointments = [
    { id: '1', doctor: 'Dr. Ahmed Al-Mutairi', time: 'Today, 3:00 PM' },
    { id: '2', doctor: 'Dr. Fatima Al-Rashed', time: 'Tomorrow, 10:30 AM' },
  ];

  const healthTips = [
    { id: '1', title: 'Stay Hydrated', icon: 'water' },
    { id: '2', title: 'Daily Exercise', icon: 'walk' },
    { id: '3', title: 'Healthy Diet', icon: 'nutrition' },
    { id: '4', title: 'Regular Checkups', icon: 'medkit' },
  ];

  const quickActions = [
    { id: '1', title: 'Book Appointment', icon: 'calendar', screen: 'Appointments' },
    { id: '2', title: 'Find Doctor', icon: 'search', screen: 'Doctors' },
    { id: '3', title: 'My Prescriptions', icon: 'document', screen: 'Prescriptions' },
    { id: '4', title: 'Lab Results', icon: 'flask', screen: 'Results' },
  ];

  const handleQuickAction = (screen) => {
    if (['Prescriptions', 'Results'].includes(screen)) {
      Alert.alert('Coming Soon', 'This feature will be available soon!');
      return;
    }
    navigation.navigate(screen);
  };

  return (
    <SafeAreaView style={styles.homeContainer}>
      <ScrollView showsVerticalScrollIndicator={false}>
        {/* Header Section */}
        <View style={styles.header}>
          <View>
            <Text style={styles.greeting}>Good Morning</Text>
            <Text style={styles.userName}>Shatha Al-Zahrani</Text>
          </View>
          <TouchableOpacity onPress={() => navigation.navigate('Profile')}>
            <Icon name="person-circle" size={40} color="#2a7fba" />
          </TouchableOpacity>
        </View>

        {/* Quick Actions Grid */}
        <View style={styles.gridContainer}>
          {quickActions.map((action) => (
            <TouchableOpacity 
              key={action.id} 
              style={styles.gridItem}
              onPress={() => handleQuickAction(action.screen)}
            >
              <Icon name={action.icon} size={30} color="#2a7fba" />
              <Text style={styles.gridText}>{action.title}</Text>
            </TouchableOpacity>
          ))}
        </View>

        {/* Upcoming Appointments */}
        <Text style={styles.sectionTitle}>Upcoming Appointments</Text>
        <FlatList
          horizontal
          data={upcomingAppointments}
          keyExtractor={(item) => item.id}
          showsHorizontalScrollIndicator={false}
          renderItem={({ item }) => (
            <View style={styles.appointmentCard}>
              <Text style={styles.doctorName}>{item.doctor}</Text>
              <Text style={styles.appointmentTime}>{item.time}</Text>
              <TouchableOpacity 
                style={styles.detailsButton}
                onPress={() => navigation.navigate('Appointments')}
              >
                <Text style={styles.detailsButtonText}>View Details</Text>
              </TouchableOpacity>
            </View>
          )}
        />

        {/* Health Tips */}
        <Text style={styles.sectionTitle}>Health Tips</Text>
        <FlatList
          horizontal
          data={healthTips}
          keyExtractor={(item) => item.id}
          showsHorizontalScrollIndicator={false}
          renderItem={({ item }) => (
            <View style={styles.tipCard}>
              <Icon name={item.icon} size={40} color="#2a7fba" />
              <Text style={styles.tipTitle}>{item.title}</Text>
            </View>
          )}
        />

        {/* Emergency Section */}
        <TouchableOpacity 
          style={styles.emergencyCard}
          onPress={() => Alert.alert('Emergency', 'Contacting nearest hospital...')}
        >
          <View>
            <Text style={styles.emergencyTitle}>Emergency Assistance</Text>
            <Text style={styles.emergencyText}>Immediate medical help</Text>
          </View>
          <Icon name="alert-circle" size={40} color="#dc3545" />
        </TouchableOpacity>
      </ScrollView>
    </SafeAreaView>
  );
};
// Doctors List Screen
const DoctorsScreen = () => {
  const doctors = [
    { id: '1', name: 'Dr. Ahmed Al-Mutairi', specialty: 'Cardiologist' },
    { id: '2', name: 'Dr. Fatima Al-Rashed', specialty: 'Dermatologist' },
    { id: '3', name: 'Dr. Shatha Al-Zahrani', specialty: 'Obstetrics and gynaecology' },
    { id: '4', name: 'Dr. Joud AL-Otaibi', specialty: 'Emergency medicine' },
    { id: '5', name: 'Dr. Asma Al-Zahrani', specialty: 'Anesthesiology' },
    { id: '6', name: 'Dr. Razan Alnassar', specialty: 'Family medicine' },
    { id: '7', name: 'Dr. Amjad Rashid', specialty: 'Medical genetics' },
    { id: '8', name: 'Dr. Atheer Al-Harbi', specialty: 'Facial Plastic Surgery' },
    { id: '9', name: 'Dr. Waad Nasser', specialty: 'Oncologist' },
    { id: '10', name: 'Dr. Yasser Al-Mutairi', specialty: 'Surgery' },
    { id: '11', name: 'Dr. Majed Al-Otaibi', specialty: 'Intensive care medicine' },
    { id: '12', name: 'Dr. Fahad Al-Harbi', specialty: 'Immunology' },

  ];

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Doctors List</Text>
      <FlatList
        data={doctors}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.card}>
            <Text style={styles.name}>{item.name}</Text>
            <Text>{item.specialty}</Text>
          </View>
        )}
      />
    </View>
  );
};

// Appointments Screen
const AppointmentsScreen = () => {
  const [appointments, setAppointments] = useState([
    { id: '1', doctor: 'Dr. Ahmed Al-Mutairi', date: 'March 25, 2025' },
    { id: '2', doctor: 'Dr. Fatima Al-Rashed', date: 'March 28, 2025' },
    { id: '3', doctor: 'Dr. Shatha Al-Zahrani', date: 'March 30, 2025' },
    { id: '4', doctor: 'Dr. Joud AL-Otaibi', date: 'April 2, 2025' },
    { id: '5', doctor: 'Dr. Asma Al-Zahrani', date: 'April 5, 2025' },
    { id: '6', doctor: 'Dr. Razan Alnassar', date: 'April 10, 2025' },
    { id: '7', doctor: 'Dr. Amjad Rashid', date: 'April 15, 2025' },
    { id: '8', doctor: 'Dr. Atheer Al-Harbi', date: 'April 16, 2025' },
    { id: '9', doctor: 'Dr. Waad Nasser', date: 'April 17, 2025' },
    { id: '10', doctor: 'Dr. Yasser Al-Mutairi', date: 'April 28, 2025' },

    { id: '11', doctor: 'Dr. Majed Al-Otaibi', date: 'May 28, 2025' },
    { id: '12', doctor: 'Dr. Fahad Al-Harbi', date: 'May 1, 2025' },
  ]);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Your Appointments</Text>
      <FlatList
        data={appointments}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.card}>
            <Text style={styles.name}>{item.doctor}</Text>
            <Text>Date: {item.date}</Text>
          </View>
        )}
      />
      <Button title="Book New Appointment" onPress={() => alert('Booking Feature Coming Soon!')} />
    </View>
  );
};

// Profile Screen

const ProfileScreen = () => {
  const user = {
    name: 'Shatha Hassan Al-Zahrani',
    age: 22,
    nationalId: '11202XXXXX',
    phone: '+966 555 555 555',
    bloodType: 'O+',
    medicalConditions: ['Hypertension', 'Type 2 Diabetes'],
    allergies: ['Penicillin', 'Peanuts'],
    profileImage: null
  };

  return (
    <SafeAreaView style={styles.profileContainer}>
      {/* Profile Header */}
      <View style={styles.profileHeader}>
        <View style={styles.avatarContainer}>
          {user.profileImage ? (
            <Image source={{ uri: user.profileImage }} style={styles.avatar} />
          ) : (
            <Icon name="person-circle" size={80} color="#2a7fba" />
          )}
        </View>
        <Text style={styles.userName}>{user.name}</Text>
        <Text style={styles.userAge}>Age: {user.age}</Text>
      </View>

      {/* Personal Information Card */}
      <View style={styles.infoCard}>
        <Text style={styles.cardTitle}>Personal Information</Text>
        
        <View style={styles.infoRow}>
          <Icon name="id-card" size={20} color="#666" />
          <Text style={styles.infoLabel}>National ID:</Text>
          <Text style={styles.infoValue}>{user.nationalId}</Text>
        </View>

        <View style={styles.infoRow}>
          <Icon name="phone-portrait" size={20} color="#666" />
          <Text style={styles.infoLabel}>Phone:</Text>
          <Text style={styles.infoValue}>{user.phone}</Text>
        </View>
      </View>

      {/* Medical Information Card */}
      <View style={styles.infoCard}>
        <Text style={styles.cardTitle}>Medical Information</Text>
        
        <View style={styles.infoRow}>
          <Icon name="water" size={20} color="#666" />
          <Text style={styles.infoLabel}>Blood Type:</Text>
          <Text style={[styles.infoValue, styles.bloodType]}>{user.bloodType}</Text>
        </View>

        <View style={styles.medicalSection}>
          <View style={styles.iconLabel}>
            <Icon name="warning" size={20} color="#666" />
            <Text style={styles.sectionSubtitle}>Medical Conditions</Text>
          </View>
          {user.medicalConditions.map((condition, index) => (
            <Text key={index} style={styles.medicalItem}>• {condition}</Text>
          ))}
        </View>

        <View style={styles.medicalSection}>
          <View style={styles.iconLabel}>
            <Icon name="alert-circle" size={20} color="#666" />
            <Text style={styles.sectionSubtitle}>Allergies</Text>
          </View>
          {user.allergies.map((allergy, index) => (
            <Text key={index} style={styles.medicalItem}>• {allergy}</Text>
          ))}
        </View>
      </View>

      {/* Emergency Button */}
      <TouchableOpacity style={styles.emergencyButton}>
        <Icon name="medkit" size={24} color="white" />
        <Text style={styles.emergencyButtonText}>Emergency Contact</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
};



// Tab Navigator
const Tab = createBottomTabNavigator();

const MainTabNavigator = () => (
  <Tab.Navigator
    screenOptions={{
      headerShown: false,
      tabBarActiveTintColor: '#2a7fba',
    }}
  >
    <Tab.Screen 
      name="Home" 
      component={HomeScreen} 
      options={{ 
        tabBarIcon: ({ color, size }) => <Icon name="home" color={color} size={size} />
      }} 
    />
    <Tab.Screen 
      name="Doctors" 
      component={DoctorsScreen} 
      options={{ 
        tabBarIcon: ({ color, size }) => <Icon name="medkit" color={color} size={size} />
      }} 
    />
    <Tab.Screen 
      name="Appointments" 
      component={AppointmentsScreen} 
      options={{ 
        tabBarIcon: ({ color, size }) => <Icon name="calendar" color={color} size={size} />
      }} 
    />
    <Tab.Screen 
      name="Profile" 
      component={ProfileScreen} 
      options={{ 
        tabBarIcon: ({ color, size }) => <Icon name="person" color={color} size={size} />
      }} 
    />
  </Tab.Navigator>
);

// Stack Navigator
const Stack = createNativeStackNavigator();


export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen 
          name="Login" 
          component={LoginScreen} 
          options={{ headerShown: false }} 
        />
        <Stack.Screen 
          name="MainApp" 
          component={MainTabNavigator} 
          options={{ headerShown: false }} 
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}



const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    justifyContent: 'center',
    padding: 20,
  },
  innerContainer: {
    padding: 20,
  },
  logo: {
    fontWeight: 'bold',
    fontSize: 30,
    color: '#2a7fba',
    marginBottom: 40,
    textAlign: 'center',
  },
  inputView: {
    width: '100%',
    backgroundColor: '#e8f4f8',
    borderRadius: 25,
    height: 50,
    marginBottom: 20,
    justifyContent: 'center',
    padding: 20,
  },
  inputText: {
    height: 50,
    color: '#003f5c',
  },
  loginBtn: {
    width: '100%',
    backgroundColor: '#2a7fba',
    borderRadius: 25,
    height: 50,
    alignItems: 'center',
    justifyContent: 'center',
    marginTop: 20,
    marginBottom: 10,
  },
  loginText: {
    color: 'white',
    fontWeight: 'bold',
  },
  signupContainer: {
    flexDirection: 'row',
    justifyContent: 'center',
    marginTop: 20,
  },
  signupText: {
    color: '#003f5c',
  },
  signupLink: {
    color: '#2a7fba',
    fontWeight: 'bold',
  },
  title: { 
    fontSize: 22, 
    fontWeight: 'bold', 
    marginBottom: 20 
  },
  card: { 
    backgroundColor: '#fff', 
    padding: 15, 
    marginVertical: 5, 
    borderRadius: 10, 
    width: '100%',
    elevation: 2,
  },
  name: { 
    fontSize: 18, 
    fontWeight: 'bold' 
  },
  profileContainer: {
    flex: 1,
    backgroundColor: '#f8f9fa',
  },
  profileHeader: {
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#2a7fba',
    borderBottomLeftRadius: 30,
    borderBottomRightRadius: 30,
  },
  avatarContainer: {
    marginBottom: 15,
  },
  avatar: {
    width: 100,
    height: 100,
    borderRadius: 50,
  },
  userName: {
    fontSize: 22,
    fontWeight: 'bold',
    color: 'white',
    marginBottom: 5,
  },
  userAge: {
    fontSize: 16,
    color: '#e3f2fd',
  },
  infoCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    margin: 15,
    elevation: 3,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  cardTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#2a7fba',
    marginBottom: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
    paddingBottom: 10,
  },
  infoRow: {
    flexDirection: 'row',
    alignItems: 'center',
    marginVertical: 8,
  },
  infoLabel: {
    color: '#666',
    marginLeft: 10,
    marginRight: 15,
    width: 100,
  },
  infoValue: {
    flex: 1,
    color: '#333',
    fontWeight: '500',
  },
  medicalSection: {
    marginTop: 15,
  },
  iconLabel: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 10,
  },
  sectionSubtitle: {
    fontSize: 16,
    fontWeight: '600',
    color: '#444',
    marginLeft: 10,
  },
  medicalItem: {
    color: '#666',
    marginLeft: 30,
    marginVertical: 3,
  },
  bloodType: {
    backgroundColor: '#ffe5e5',
    paddingVertical: 3,
    paddingHorizontal: 8,
    borderRadius: 5,
    color: '#c00',
  },
  emergencyButton: {
    flexDirection: 'row',
    backgroundColor: '#dc3545',
    padding: 15,
    borderRadius: 10,
    margin: 20,
    justifyContent: 'center',
    alignItems: 'center',
    elevation: 3,
  },
  emergencyButtonText: {
    color: 'white',
    fontWeight: 'bold',
    marginLeft: 10,
  },
  homeContainer: {
    flex: 1,
    backgroundColor: '#f8f9fa',
    padding: 15,
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  greeting: {
    fontSize: 18,
    color: '#666',
  },
  userName: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#2a7fba',
  },
  gridContainer: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    marginBottom: 20,
  },
  gridItem: {
    width: '48%',
    backgroundColor: 'white',
    borderRadius: 10,
    padding: 20,
    marginBottom: 15,
    alignItems: 'center',
    elevation: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  gridText: {
    marginTop: 10,
    color: '#333',
    fontWeight: '500',
    textAlign: 'center',
  },
  sectionTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#2a7fba',
    marginVertical: 15,
  },
  appointmentCard: {
    backgroundColor: 'white',
    borderRadius: 10,
    padding: 15,
    marginRight: 15,
    width: 250,
    elevation: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  doctorName: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
  },
  appointmentTime: {
    color: '#666',
    marginVertical: 10,
  },
  detailsButton: {
    backgroundColor: '#2a7fba',
    padding: 8,
    borderRadius: 5,
    alignSelf: 'flex-start',
  },
  detailsButtonText: {
    color: 'white',
    fontSize: 12,
  },
  tipCard: {
    backgroundColor: 'white',
    borderRadius: 10,
    padding: 20,
    marginRight: 15,
    alignItems: 'center',
    width: 150,
    elevation: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  tipTitle: {
    marginTop: 10,
    color: '#333',
    fontWeight: '500',
    textAlign: 'center',
  },
  emergencyCard: {
    backgroundColor: '#ffe5e5',
    borderRadius: 10,
    padding: 20,
    marginVertical: 15,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    elevation: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  emergencyTitle: {
    color: '#dc3545',
    fontSize: 18,
    fontWeight: 'bold',
  },
  emergencyText: {
    color: '#666',
    marginTop: 5,
  },
});
