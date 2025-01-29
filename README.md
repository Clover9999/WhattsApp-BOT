import pywhatkit as kit


# Número do destinatário (incluir o código do país, por exemplo, para o Brasil: +55)
numero_destinatario = '+5511979692523'  # Substitua pelo número de telefone

# Mensagem a ser enviada
mensagem = "Olá! Esta é uma mensagem automática do bot."

# Usando pywhatkit para enviar a mensagem imediatamente
kit.sendwhatmsg_instantly(numero_destinatario, mensagem)


print("Mensagem enviada com sucesso!")
