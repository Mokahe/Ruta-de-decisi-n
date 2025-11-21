import streamlit as st
import pandas as pd

# --- DATOS DE LAS PRUEBAS ---
# Incluimos la tabla generada con la información completa
data = {
    'Prueba': [
        't de Student para muestras independientes',
        'U de Mann Whitney',
        'ANOVA de un factor',
        'Kruskal-Wallis',
        't de Student para muestras relacionadas',
        'Wilcoxon (Rangos con signo)',
        'Correlación de Pearson',
        'Correlación de Spearman',
        'Regresión Lineal Simple',
        'Chi-cuadrada ($\chi^2$)',
    ],
    'Paramétrica o no paramétrica': [
        'Paramétrica', 'No paramétrica', 'Paramétrica', 'No paramétrica', 
        'Paramétrica', 'No paramétrica', 'Paramétrica', 'No paramétrica', 
        'Paramétrica', 'No paramétrica'
    ],
    'Objetivo que persigue': [
        'Comparar medias de **dos grupos independientes**.',
        'Comparar distribuciones (o medianas) de **dos grupos independientes** (no paramétrica).',
        'Comparar medias de **tres o más grupos independientes**.',
        'Comparar distribuciones (o medianas) de **tres o más grupos independientes** (no paramétrica).',
        'Comparar medias de **dos mediciones en el mismo grupo** (muestras relacionadas).',
        'Comparar distribuciones (o medianas) de **dos mediciones en el mismo grupo** (muestras relacionadas, no paramétrica).',
        'Medir la **fuerza y dirección** de la relación **lineal** entre dos variables cuantitativas.',
        'Medir la **fuerza y dirección** de la relación **monotónica** (no necesariamente lineal) entre dos variables.',
        '**Predecir** el valor de una variable a partir de otra y modelar su relación lineal.',
        'Evaluar la **asociación** entre **dos variables categóricas**.',
    ],
    'Criterios/supuestos clave': [
        'Normalidad y Homogeneidad de Varianzas.',
        'Independencia de las observaciones.',
        'Normalidad y Homogeneidad de Varianzas (Homocedasticidad).',
        'Independencia de las observaciones.',
        'Normalidad de la distribución de las **diferencias**.',
        'Las observaciones dentro de los pares son dependientes.',
        'Linealidad, Normalidad bivariada, Homoscedasticidad.',
        'Variables ordinales o cuantitativas, relación monotónica.',
        'Linealidad, Normalidad de los residuos, Homoscedasticidad.',
        'Variables categóricas, Frecuencias esperadas > 5.',
    ]
}

df_pruebas = pd.DataFrame(data)

# --- FUNCIÓN PRINCIPAL DEL DASHBOARD ---

def decision_tree_app():
    st.set_page_config(
        page_title="Asistente de Selección de Pruebas Estadísticas",
        layout="wide",
        initial_sidebar_state="expanded"
    )
    
    st.title("🧠 Asistente de Selección de Pruebas Estadísticas")
    st.markdown("Utiliza este flujo de decisión para elegir la prueba estadística más adecuada para tu análisis.")

    st.sidebar.header("Ruta de Decisión 🗺️")
    
    # --- PASO 1: OBJETIVO ---
    st.header("1. ¿Cuál es el objetivo principal de tu análisis?")
    objetivo = st.radio(
        "Elige la opción que mejor describe tu objetivo:",
        ('Comparar Grupos', 'Evaluar la Relación/Asociación entre Variables', 'Predecir un valor'),
        key='objetivo_key'
    )
    
    st.markdown("---")
    
    # --- RAMA 1: COMPARAR GRUPOS ---
    if objetivo == 'Comparar Grupos':
        st.header("2. Comparación: ¿Cuántos grupos/mediciones tienes?")
        grupos = st.radio(
            "Selecciona el número de grupos o mediciones:",
            ('Dos grupos independientes (e.g., Hombres vs Mujeres)', 
             'Tres o más grupos independientes (e.g., Tratamiento A vs B vs C)',
             'Dos mediciones del mismo grupo (Muestras relacionadas/dependientes - e.g., Antes vs Después)'),
            key='grupos_key'
        )
        
        if grupos in ['Dos grupos independientes (e.g., Hombres vs Mujeres)', 'Tres o más grupos independientes (e.g., Tratamiento A vs B vs C)']:
            st.header("3. Comparación: ¿Qué tipo de datos tienes y qué supuestos cumples?")
            tipo_datos_comp = st.radio(
                "¿La variable dependiente es cuantitativa y cumple los supuestos de normalidad y homogeneidad de varianzas?",
                ('Sí (Datos paramétricos)', 'No (Datos no paramétricos)'),
                key='tipo_datos_comp_key'
            )
            
            if grupos == 'Dos grupos independientes (e.g., Hombres vs Mujeres)':
                if tipo_datos_comp == 'Sí (Datos paramétricos)':
                    st.success("**✅ PRUEBA SUGERIDA: t de Student para muestras independientes**")
                    mostrar_detalles('t de Student para muestras independientes')
                else:
                    st.success("**✅ PRUEBA SUGERIDA: U de Mann Whitney**")
                    mostrar_detalles('U de Mann Whitney')
                    
            elif grupos == 'Tres o más grupos independientes (e.g., Tratamiento A vs B vs C)':
                if tipo_datos_comp == 'Sí (Datos paramétricos)':
                    st.success("**✅ PRUEBA SUGERIDA: ANOVA de un factor**")
                    mostrar_detalles('ANOVA de un factor')
                else:
                    st.success("**✅ PRUEBA SUGERIDA: Kruskal-Wallis**")
                    mostrar_detalles('Kruskal-Wallis')
                    
        elif grupos == 'Dos mediciones del mismo grupo (Muestras relacionadas/dependientes - e.g., Antes vs Después)':
            st.header("3. Comparación: ¿Qué tipo de datos tienes y qué supuestos cumples?")
            tipo_datos_rel = st.radio(
                "¿La variable dependiente es cuantitativa y la distribución de las diferencias cumple el supuesto de normalidad?",
                ('Sí (Datos paramétricos)', 'No (Datos no paramétricos)'),
                key='tipo_datos_rel_key'
            )
            
            if tipo_datos_rel == 'Sí (Datos paramétricos)':
                st.success("**✅ PRUEBA SUGERIDA: t de Student para muestras relacionadas**")
                mostrar_detalles('t de Student para muestras relacionadas')
            else:
                st.success("**✅ PRUEBA SUGERIDA: Wilcoxon (Rangos con signo)**")
                mostrar_detalles('Wilcoxon (Rangos con signo)')

    # --- RAMA 2: EVALUAR RELACIÓN/ASOCIACIÓN ---
    elif objetivo == 'Evaluar la Relación/Asociación entre Variables':
        st.header("2. Relación: ¿Cuál es el tipo de datos de tus variables principales?")
        tipo_relacion = st.radio(
            "Selecciona el tipo de variables a relacionar:",
            ('Ambas variables son **Cuantitativas**', 'Ambas variables son **Categóricas** (Nominales u Ordinales)'),
            key='tipo_relacion_key'
        )
        
        if tipo_relacion == 'Ambas variables son **Cuantitativas**':
            st.header("3. Relación Cuantitativa: ¿Asumes linealidad y normalidad?")
            supuestos_cuant = st.radio(
                "¿La relación es lineal y tus datos cumplen los supuestos paramétricos (e.g., normalidad)?",
                ('Sí (Relación Lineal / Paramétrica)', 'No (Relación Monotónica / No Paramétrica)'),
                key='supuestos_cuant_key'
            )
            
            if supuestos_cuant == 'Sí (Relación Lineal / Paramétrica)':
                st.success("**✅ PRUEBA SUGERIDA: Correlación de Pearson**")
                mostrar_detalles('Correlación de Pearson')
            else:
                st.success("**✅ PRUEBA SUGERIDA: Correlación de Spearman**")
                mostrar_detalles('Correlación de Spearman')
                
        elif tipo_relacion == 'Ambas variables son **Categóricas** (Nominales u Ordinales)':
            st.success("**✅ PRUEBA SUGERIDA: Chi-cuadrada ($\chi^2$)**")
            mostrar_detalles('Chi-cuadrada ($\chi^2$)')
            
    # --- RAMA 3: PREDECIR UN VALOR ---
    elif objetivo == 'Predecir un valor':
        st.header("2. Predicción: ¿Qué tipo de variables tienes?")
        tipo_prediccion = st.radio(
            "¿Tu variable dependiente (a predecir) es cuantitativa y tienes una variable predictora (cuantitativa)?",
            ('Sí (Modelo Lineal Simple)', 'Otro tipo de predicción (Fuera del alcance de este asistente simple)'),
            key='tipo_prediccion_key'
        )
        
        if tipo_prediccion == 'Sí (Modelo Lineal Simple)':
            st.success("**✅ PRUEBA SUGERIDA: Regresión Lineal Simple**")
            mostrar_detalles('Regresión Lineal Simple')
        else:
            st.info("Para otros modelos (e.g., Regresión Logística), necesitarías asistentes más complejos.")


# --- FUNCIÓN PARA MOSTRAR DETALLES DE LA PRUEBA ---

def mostrar_detalles(nombre_prueba):
    """Muestra una tarjeta con los detalles de la prueba seleccionada."""
    st.subheader(f"Detalles de la Prueba: **{nombre_prueba}**")
    
    # Buscar los datos en el DataFrame
    prueba_info = df_pruebas[df_pruebas['Prueba'] == nombre_prueba].iloc[0]
    
    # Mostrar la información en un formato de tarjeta
    with st.expander("Ver Objetivo y Criterios", expanded=True):
        col1, col2 = st.columns(2)
        
        with col1:
            st.markdown(f"**Tipo:** {prueba_info['Paramétrica o no paramétrica']}")
            st.markdown(f"**🎯 Objetivo:** {prueba_info['Objetivo que persigue']}")

        with col2:
            st.markdown("**📜 Criterios/Supuestos Clave:**")
            st.warning(f"{prueba_info['Criterios/supuestos clave']}")
            
    st.sidebar.markdown(f"**Última prueba seleccionada:** {nombre_prueba}")
    st.sidebar.markdown("---")
    st.sidebar.dataframe(df_pruebas[['Prueba', 'Paramétrica o no paramétrica']], height=250)
    
# --- EJECUTAR APP ---
if __name__ == "__main__":
    decision_tree_app()
