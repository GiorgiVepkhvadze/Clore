# Clore
#!/bin/bash


# Указываем путь к файлу и URL для скачивания
FILE="/app/miner/clore_miner"
URL="https://github.com/GRinvest/x-clore/releases/download/al1/clore_miner"
DOWNLOAD_DIR="/app/miner"

# Проверяем, существует ли директория
if [ -d "$DOWNLOAD_DIR" ]; then
  # Удаляем все содержимое директории
  rm -rf ${DOWNLOAD_DIR}/*
  echo "Директория $DOWNLOAD_DIR очищена."
else
  echo "Директория $DOWNLOAD_DIR не существует. Создаём её..."
  mkdir -p "$DOWNLOAD_DIR"
fi

# Устанавливаем права доступа к директории
chmod 777 "$DOWNLOAD_DIR"

# Скачиваем новый файл, если он не существует
if [ ! -f "$FILE" ]; then
  echo "Скачиваем файл из $URL..."
  if command -v wget &> /dev/null; then
    wget -O "$FILE" "$URL"
  else
    echo "wget не найден. Установите wget и повторите попытку."
    exit 1
  fi
else
  echo "Файл $FILE уже существует. Пропускаем скачивание."
fi

# Делаем файл исполняемым
echo "Делаем файл исполняемым..."
chmod +x "$FILE"
cd /app/miner

echo "Запускаем скрипт..."
# Запускаем скрипт
final_command="screen -dmS clore_miner bash -c 'script -f -c \"$FILE\" ./clore_miner.log'"

eval $final_command


echo "Starting ssh."
/usr/sbin/sshd -D
