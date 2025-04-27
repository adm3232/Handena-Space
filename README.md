App.js

import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { StripeProvider } from '@stripe/stripe-react-native';
import HomeScreen from './screens/HomeScreen';
import CursosScreen from './screens/CursosScreen';
import ProdutosScreen from './screens/ProdutosScreen';
import CadastroScreen from './screens/CadastroScreen';
import PagamentoScreen from './screens/PagamentoScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <StripeProvider publishableKey="SUA_CHAVE_PUBLICA_STRIPE">
      <NavigationContainer>
        <Stack.Navigator initialRouteName="Home">
          <Stack.Screen name="Home" component={HomeScreen} />
          <Stack.Screen name="Cursos" component={CursosScreen} />
          <Stack.Screen name="Produtos" component={ProdutosScreen} />
          <Stack.Screen name="Cadastro" component={CadastroScreen} />
          <Stack.Screen name="Pagamento" component={PagamentoScreen} />
        </Stack.Navigator>
      </NavigationContainer>
    </StripeProvider>
  );
}


---

/screens/HomeScreen.js

import React from 'react';
import { View, Text, Button } from 'react-native';

export default function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text style={{ fontSize: 24, fontWeight: 'bold' }}>Handena Business</Text>
      <Button title="Ver Cursos" onPress={() => navigation.navigate('Cursos')} />
      <Button title="Ver Produtos" onPress={() => navigation.navigate('Produtos')} />
      <Button title="Cadastro" onPress={() => navigation.navigate('Cadastro')} />
      <Button title="Pagamento" onPress={() => navigation.navigate('Pagamento')} />
    </View>
  );
}


---

/screens/CursosScreen.js

import React from 'react';
import { View, Text, Button } from 'react-native';
import { launchImageLibrary } from 'react-native-image-picker';
import storage from '@react-native-firebase/storage';

export default function CursosScreen() {
  const uploadFile = () => {
    launchImageLibrary({ mediaType: 'mixed' }, async (response) => {
      if (response.assets) {
        const fileUri = response.assets[0].uri;
        const fileName = fileUri.substring(fileUri.lastIndexOf('/') + 1);
        const reference = storage().ref(`cursos/${fileName}`);

        try {
          await reference.putFile(fileUri);
          alert('Upload concluído!');
        } catch (error) {
          console.error(error);
          alert('Erro no upload');
        }
      }
    });
  };

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text style={{ fontSize: 20 }}>Lista de Cursos</Text>
      <Button title="Fazer Upload de Curso" onPress={uploadFile} />
    </View>
  );
}


---

/screens/ProdutosScreen.js

import React from 'react';
import { View, Text } from 'react-native';

export default function ProdutosScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text style={{ fontSize: 20 }}>Lista de Produtos Físicos e Digitais</Text>
    </View>
  );
}


---

/screens/CadastroScreen.js

import React, { useState } from 'react';
import { View, Text, TextInput, Button } from 'react-native';

export default function CadastroScreen() {
  const [nome, setNome] = useState('');
  const [telefone, setTelefone] = useState('');
  const [bilhete, setBilhete] = useState('');

  const cadastrar = () => {
    if (bilhete.length < 14) {
      alert('Número de Bilhete inválido!');
      return;
    }
    alert(`Usuário ${nome} cadastrado com sucesso!`);
  };

  return (
    <View style={{ flex: 1, justifyContent: 'center', padding: 20 }}>
      <Text style={{ fontSize: 20 }}>Cadastro</Text>
      <TextInput
        placeholder="Nome completo"
        value={nome}
        onChangeText={setNome}
        style={{ borderWidth: 1, padding: 10, marginVertical: 10 }}
      />
      <TextInput
        placeholder="Telefone"
        keyboardType="phone-pad"
        value={telefone}
        onChangeText={setTelefone}
        style={{ borderWidth: 1, padding: 10, marginVertical: 10 }}
      />
      <TextInput
        placeholder="Número do Bilhete de Identidade"
        value={bilhete}
        onChangeText={setBilhete}
        style={{ borderWidth: 1, padding: 10, marginVertical: 10 }}
      />
      <Button title="Cadastrar" onPress={cadastrar} />
    </View>
  );
}


---

/screens/PagamentoScreen.js

import React from 'react';
import { View, Button } from 'react-native';
import { CardField, useStripe } from '@stripe/stripe-react-native';

export default function PagamentoScreen() {
  const { confirmPayment } = useStripe();

  const pagar = async () => {
    const clientSecret = 'CLIENT_SECRET_DO_SEU_BACKEND';
    const { error, paymentIntent } = await confirmPayment(clientSecret, {
      paymentMethodType: 'Card',
    });

    if (error) {
      alert(`Erro: ${error.message}`);
    } else if (paymentIntent) {
      alert('Pagamento realizado com sucesso!');
    }
  };

  return (
    <View style={{ flex: 1, justifyContent: 'center', padding: 20 }}>
      <CardField
        postalCodeEnabled={false}
        placeholder={{ number: '4242 4242 4242 4242' }}
        cardStyle={{ backgroundColor: '#FFFFFF', textColor: '#000000' }}
        style={{ width: '100%', height: 50, marginVertical: 30 }}
      />
      <Button title="Pagar" onPress={pagar} />
    </View>
  );
}


---

Pacotes que você precisa instalar:

npm install @react-navigation/native @react-navigation/native-stack
npm install @stripe/stripe-react-native
npm install @react-native-firebase/app @react-native-firebase/storage
npm install react-native-image-pick 




 