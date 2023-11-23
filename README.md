#Start
documentation can be found https://reactnative.dev/

There 2 options:
- Expo CLI (default) - Third-part service(free). Provide managed aap development. (Easy development). Can swicht to ReactNative any time
 run `sudo npm  install -g expo-cli`
 run `npx create-expo-app AwesomeProject`      
      `cd AwesomeProject`
      `npx expo start`
- ReactNative CLI. Tool provided to ReactNative tool. 

##Running on real device

Install Expo App on your device
run `npx expo start`
scan barCode by phone. And grant the all needed permission to Expo App

##Running on emulator
install Android studio (Linux or Windows)
or 
Xcode (only for iOS)

## Add navigation
Documentations is here https://reactnavigation.org/

- run `npm install @react-navigation/native`
- if useExpo 
    run `npx expo install react-native-screens react-native-safe-area-context`
-use App.js 
    - import {NavigationContainer} and wrap it around component that has to use it.
    - use one of supported Navigators (Stack, NativeStack, Draws,...})
    - install needed Navigator. For example Stack
    run `npm install @react-navigation/native-stack`
    - in App.js
      - import { createNativeStackNavigator } from '@react-navigation/native-stack';
      - create const Stack = createNativeStackNavigator();
      - <Stack.Navigator>
         <Stack.Screen/>
       </Stack.Navigator>
       - Config the Stack.Screen
          <Stack.Screen name="MealsCategories" component = {CategoriesScreens}/>
       - {navigation} if provided to component registered as screen in Stack (App.js)
       - to have access to navigation in any component import {useNavigation} from '@react-navigation/native'
       - use navigation.navigate('MealsOverview') (argument is the name of component to navigate to - the samenameas in App.js in <Stack)
      
### Use navigation
 navigation.navigate('MealsOverview', {
                categoryId: itemDate.item.id, // second argument - is theobjectthat can be pass to target screen
            }) 
            
 the target screen (which is registeed in App.js) obtaines the {route} props
 const MealsOverviewScreen = ({route}) =>{.....} and route.params - the objected past by navigation

OR 
use import {useRoute} from '@react-navigation/native'
const route = useRoute();
const pastParams = route.params.pastParams;
in any component

- Adding button to the header:
with the option on the <Stack.Screen>
options={{
        headerRight:()=>{
                            return(
                              <Button title="*"/>
                            )
                        }
                    }}
OR directly in the component

###Drawer Navigation
- install `npm install @react-navigation/drawer`
- if using Expo run `npx expo install react-native-gesture-handler react-native-reanimated`
- in App.js add 
    - import { createDrawerNavigator } from '@react-navigation/drawer';
    -   const Drawer = createDrawerNavigator();
 
-   function MyDrawer() {
   return (
     <Drawer.Navigator>
       <Drawer.Screen name="Feed" component={Feed} />
       <Drawer.Screen name="Article" component={Article} />
     </Drawer.Navigator>
   );
 }
 
To use drawer in for example button

const openDrawer = ()=>{
navigation.toggleDrawer(); //navigation is pass automatically to ScreenComponent or use useNavigation()
}
<Button onPress={openDrawer} title="openDrawer">

##Bottom tabs
- install 'npm install @react-navigation/bottom-tabs'
- add in App.js
    - import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
    - const Tab = createBottomTabNavigator();
    -function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}
### ICons
import {Ionicons} from '@expo/vector-icons'

##Context API
- create store folder and context subfolders.
- create Fafavorites-context.js
    - import {createContext} from 'react';         
    - const FavoritesContext = createContext();
    - create function FavoriteContextProvider ({children}) {             
                 return <FavoritesContext.Provider>{children}</FavoritesContext.Provider>
             }
             export default FavoriteContextProvider
- wrap components that will use context with <FavoriteContextProvider></FavoriteContextProvider>
- create initState, and all function we need
- Use context
  - in component we need context 
    - import { useContext } from 'react';
    - const ctx = useContext();
    
##Redux Toolkit
- install `npm install @reduxjs/toolkit`
- install `npm install react-redux`
- create store folder redux subfolders. 
- create store.js
    - import {configureStore} from '@reduxjs/toolkit'
    - wrap the App.js with  <Provider store={store}> and provide store;
    - import { Provider } from 'react-redux';
    - import {store} from './store/redux/store';
    - create extra store file (favorites.js);
        - Slice import {createSlice} from '@reduxjs/toolkit';
        - create initials state and reducers
        - export default the reducers
        - export all actions
    - in store.js import favorites from './favorites';
    - reducer: {
              favoriteMeals: favorites  //to use ut with the name 'favoriteMeals' over the App
          }
####Use redux store
- In the component wee need data from store import {useSelector} from 'react-redux';
     const favoriteMealsIds =  useSelector((state)=> state.favoriteMeals.ids);
- To dispatch action  import {useDispatch} from 'react-redux';
         - const dispatch = useDispatch();
         - import {action1, action2} from '../store/redux/favorites';
         - dispatch(action1({id: mealId}))



## Build and Publish App

Publishing - put it in Google or AppStore

### With Expo
- Configuring Projects Build App Binaries
- build on Expo's clouds Services
- Submit App to stores 

Documentation
   - https://docs.expo.dev/eas/   
   - https://docs.expo.dev/versions/latest/config/app/     - App name, unique identifier, version
   - https://docs.expo.dev/build-reference/variables/      - Environment Variables
   - https://docs.expo.dev/tutorial/configuration/         - Icons & Splash Screen 

#### Config settings
The settings are in app.json
#### Build
Documentation https://docs.expo.dev/build/setup/

- create App
- create Expo user Account
- Install the latest EAS CLI.
Run `npm install -g eas-cli`
- login to eas account. Run `eas login`
- Config project. Run `eas build:configure`. This will add eas.json
- Build the installable APK  to test it for:
#### Android 
Documentation https://docs.expo.dev/build-reference/apk/
   
- Modify eas.json 
    - for Android Add {
                        "build": {
                          "preview": {
                            "android": {
                              "buildType": "apk"
                            }
                          },
                          "preview2": {
                            "android": {
                              "gradleCommand": ":app:assembleRelease"
                            }
                          },
                          "preview3": {
                            "developmentClient": true
                          },
                          "production": {}
                        }
                      }
 
    - run `eas build -p android --profile preview` to build for android
    - add unique App identifier
    - add Android Keystore
    
#### iOS 
Documentation https://docs.expo.dev/build-reference/simulators/   

- Modify eas.json 
{
  "build": {
    "preview": {
      "ios": {
        "simulator": true
      }
    },
    "production": {}
  }
}
- run `eas build -p ios --profile preview`

    
#### Build the production
- You need Google Play account or Apple Acount.
- 
    - Android run `eas build --platform android`    
    - iOs run `eas build --platform ios`
    - for all run `eas build --platform all`
    - submit APP tot store with expo eas https://docs.expo.dev/submit

#### Without Expo
- Configuring Projects Build App Binaries 
- Build App locally
- Submit App to stores 

### Configuration for production
- Permissions
- App name, unique identifier, version
- Environment Variables
- Icons & Splash Screen 

###Notifications

####Local notification 
 Is the notification that are trigered by the installed app, for the local device, NOT sent other user or devices
 

