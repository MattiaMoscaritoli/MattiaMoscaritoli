import sqlite3

# Inizializzazione database
conn = sqlite3.connect("banca_materiali.db")
cursor = conn.cursor()
cursor.execute("""
CREATE TABLE IF NOT EXISTS materiali (
    giocatore TEXT,
    materiale TEXT,
    quantita INTEGER
)
""")
conn.commit()

# Funzioni
def aggiungi_materiale(giocatore, materiale, quantita):
    cursor.execute("""
    INSERT INTO materiali (giocatore, materiale, quantita) 
    VALUES (?, ?, ?) 
    ON CONFLICT(giocatore, materiale) 
    DO UPDATE SET quantita = quantita + ?
    """, (giocatore, materiale, quantita, quantita))
    conn.commit()
    print(f"{quantita} {materiale} aggiunti per {giocatore}.")

def visualizza_materiali(giocatore):
    cursor.execute("SELECT materiale, quantita FROM materiali WHERE giocatore = ?", (giocatore,))
    inventario = cursor.fetchall()
    print(f"Inventario di {giocatore}:")
    for materiale, quantita in inventario:
        print(f"- {materiale}: {quantita}")

# Esempio di utilizzo
aggiungi_materiale("Player1", "Diamanti", 10)
aggiungi_materiale("Player1", "Legno", 64)
visualizza_materiali("Player1")

conn.close()
