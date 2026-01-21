# 🔐 Guia de Segurança - Sistema De Boa com o Óleo

## Sistema de Autenticação

O sistema utiliza **autenticação com hash SHA-256** para proteger as senhas dos usuários.

### 📋 Credenciais Atuais

#### Usuário 1:
- **Usuário:** `admin`
- **Senha:** `sanemar@2025`

#### Usuário 2:
- **Usuário:** `sanemar`
- **Senha:** `deboaoleo123`

---

## 🔧 Como Criar Novas Credenciais

### Passo 1: Gerar o Hash da Senha

1. Abra o console do navegador (F12)
2. Cole este código substituindo `'sua_senha_aqui'` pela senha desejada:

```javascript
async function hashPassword(password) {
    const encoder = new TextEncoder();
    const data = encoder.encode(password);
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
    return hashHex;
}

// Exemplo de uso:
hashPassword('sua_senha_aqui').then(hash => console.log('Hash:', hash));
```

3. Copie o hash gerado

### Passo 2: Adicionar o Usuário

Edite o arquivo `login.html`, procure por `validUsers` e adicione:

```javascript
const validUsers = {
    'admin': '9af15b336e6a9619928537df30b2e6a2376569fcf9d7e773eccede65606529a0',
    'sanemar': 'd5f8b8c5f8ff91d9db3ffd5d7c7f6c8e4b2a1f0e9d8c7b6a5f4e3d2c1b0a9f8e7',
    'novo_usuario': 'COLE_O_HASH_AQUI', // <- Adicione aqui
};
```

---

## 🛡️ Recursos de Segurança

### 1. Hash de Senhas
- Senhas são convertidas em hash SHA-256
- Senhas originais nunca são armazenadas
- Impossível recuperar a senha original do hash

### 2. Sessão com Expiração
- Sessão expira após **24 horas**
- Token de sessão único gerado a cada login
- Logout automático após expiração

### 3. Proteção de Rotas
- Páginas admin verificam autenticação
- Redirecionamento automático se não autenticado
- Token de sessão validado em cada acesso

---

## ⚠️ Recomendações de Segurança

### Para Produção Real:

1. **Não use este sistema em produção pública**
   - O sistema atual é adequado para uso interno/local
   - Para uso público, implemente backend com PHP/Node.js

2. **Use HTTPS**
   - Sempre use HTTPS em produção
   - Evita interceptação de credenciais

3. **Backend Seguro**
   - Implemente autenticação no servidor
   - Use banco de dados seguro (MySQL, PostgreSQL)
   - Adicione rate limiting para evitar ataques

4. **Senhas Fortes**
   - Mínimo 8 caracteres
   - Inclua letras, números e símbolos
   - Não use senhas óbvias

5. **Altere as Senhas Padrão**
   - Troque as senhas de teste IMEDIATAMENTE
   - Use senhas únicas para cada usuário

---

## 🔄 Como Alterar uma Senha

1. Gere o hash da nova senha (veja Passo 1 acima)
2. Edite `login.html`
3. Substitua o hash antigo pelo novo
4. Salve o arquivo

**Exemplo:**
```javascript
// Antes
'admin': '9af15b336e6a9619928537df30b2e6a2376569fcf9d7e773eccede65606529a0',

// Depois (com nova senha)
'admin': 'SEU_NOVO_HASH_AQUI',
```

---

## 📝 Logs e Auditoria

Para adicionar logs de acesso, edite `login.html` após login bem-sucedido:

```javascript
// Registra log de acesso
console.log(`Login: ${username} em ${new Date().toISOString()}`);
localStorage.setItem('lastLogin', new Date().toISOString());
```

---

## 🚨 Em Caso de Comprometimento

Se suspeitar que as credenciais foram comprometidas:

1. **Gere novos hashes** para todas as senhas
2. **Atualize o arquivo** `login.html`
3. **Notifique todos os usuários**
4. **Limpe todas as sessões** (cada usuário deve fazer logout/login)

Para limpar todas as sessões, adicione ao código:
```javascript
localStorage.clear();
```

---

## 📞 Suporte

Para dúvidas sobre segurança ou alterações no sistema de autenticação, consulte a documentação técnica ou entre em contato com o desenvolvedor.

---

**Última atualização:** Dezembro 2025  
**Versão do Sistema:** 1.0
