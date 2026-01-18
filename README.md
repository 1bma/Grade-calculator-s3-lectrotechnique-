# Grade-calculator-s3-lectrotechnique-
Grade calculator 
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة المعدل S3 - هندسة كهروتقنية</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        const Calculator = ({ className, ...props }) => (
            <i className={`fas fa-calculator ${className}`} {...props}></i>
        );
        const BookOpen = ({ className, ...props }) => (
            <i className={`fas fa-book-open ${className}`} {...props}></i>
        );
        const TrendingUp = ({ className, ...props }) => (
            <i className={`fas fa-chart-line ${className}`} {...props}></i>
        );
        const Moon = ({ className, ...props }) => (
            <i className={`fas fa-moon ${className}`} {...props}></i>
        );
        const Sun = ({ className, ...props }) => (
            <i className={`fas fa-sun ${className}`} {...props}></i>
        );

        const GPACalculator = () => {
            const [darkMode, setDarkMode] = useState(false);
            const [grades, setGrades] = useState({
                analyse3: { td: '', exam: '' },
                analyseNum: { td: '', tp: '', exam: '' },
                electricite: { td: '', tp: '', exam: '' },
                mecanique: { td: '', exam: '' },
                ondes: { td: '', tp: '', exam: '' },
                energie: { exam: '' },
                materiaux: { exam: '' },
                dao: { exam: '' },
                python: { tp: '', exam: '' }
            });

            const subjects = [
                {
                    id: 'analyse3',
                    name: 'Analyse 3',
                    coef: 3,
                    weights: { exam: 0.6, td: 0.4 },
                    fields: ['td', 'exam']
                },
                {
                    id: 'analyseNum',
                    name: 'Analyse numérique 1',
                    coef: 3,
                    weights: { exam: 0.6, td: 0.2, tp: 0.2 },
                    fields: ['td', 'tp', 'exam']
                },
                {
                    id: 'electricite',
                    name: 'Electricité fondamentale',
                    coef: 3,
                    weights: { exam: 0.6, td: 0.2, tp: 0.2 },
                    fields: ['td', 'tp', 'exam']
                },
                {
                    id: 'mecanique',
                    name: 'Mécanique rationnelle',
                    coef: 2,
                    weights: { exam: 0.6, td: 0.4 },
                    fields: ['td', 'exam']
                },
                {
                    id: 'ondes',
                    name: 'Ondes et vibrations',
                    coef: 3,
                    weights: { exam: 0.6, td: 0.2, tp: 0.2 },
                    fields: ['td', 'tp', 'exam']
                },
                {
                    id: 'energie',
                    name: 'Energie et environnement',
                    coef: 1,
                    weights: { exam: 1.0 },
                    fields: ['exam']
                },
                {
                    id: 'materiaux',
                    name: 'Matériaux en génie électrique',
                    coef: 1,
                    weights: { exam: 1.0 },
                    fields: ['exam']
                },
                {
                    id: 'dao',
                    name: 'Dessin assisté par ordinateur',
                    coef: 1,
                    weights: { exam: 1.0 },
                    fields: ['exam']
                },
                {
                    id: 'python',
                    name: 'Programmation Python',
                    coef: 2,
                    weights: { exam: 0.6, tp: 0.4 },
                    fields: ['tp', 'exam']
                }
            ];

            const handleInputChange = (subjectId, field, value) => {
                const numValue = value === '' ? '' : Math.min(20, Math.max(0, parseFloat(value) || 0));
                setGrades(prev => ({
                    ...prev,
                    [subjectId]: {
                        ...prev[subjectId],
                        [field]: numValue
                    }
                }));
            };

            const calculateSubjectAverage = (subject) => {
                const subjectGrades = grades[subject.id];
                let total = 0;
                
                subject.fields.forEach(field => {
                    const grade = parseFloat(subjectGrades[field]) || 0;
                    total += grade * subject.weights[field];
                });
                
                return total;
            };

            const calculateGPA = () => {
                let totalPoints = 0;
                let totalCoef = 0;

                subjects.forEach(subject => {
                    const avg = calculateSubjectAverage(subject);
                    totalPoints += avg * subject.coef;
                    totalCoef += subject.coef;
                });

                return totalCoef > 0 ? (totalPoints / totalCoef).toFixed(2) : '0.00';
            };

            const getFieldLabel = (field) => {
                const labels = {
                    td: 'TD',
                    tp: 'TP',
                    exam: 'Examen'
                };
                return labels[field] || field;
            };

            const gpa = calculateGPA();
            const getGPAMention = (gpa) => {
                const numGPA = parseFloat(gpa);
                if (numGPA >= 16) return 'ممتاز';
                if (numGPA >= 14) return 'جيد جداً';
                if (numGPA >= 12) return 'جيد';
                if (numGPA >= 10) return 'مقبول';
                return 'راسب';
            };

            return (
                <div className={`min-h-screen ${darkMode ? 'bg-gray-900' : 'bg-gradient-to-br from-blue-50 to-indigo-100'} p-4 transition-colors duration-300`}>
                    <div className="max-w-6xl mx-auto">
                        {/* Header */}
                        <div className={`${darkMode ? 'bg-gray-800' : 'bg-white'} rounded-lg shadow-lg p-6 mb-6`}>
                            <div className="flex items-center justify-between">
                                <div className="flex items-center gap-3 mb-2">
                                    <Calculator className={`w-8 h-8 ${darkMode ? 'text-indigo-400' : 'text-indigo-600'}`} />
                                    <h1 className={`text-3xl font-bold ${darkMode ? 'text-white' : 'text-gray-800'}`}>حاسبة المعدل</h1>
                                </div>
                                <button
                                    onClick={() => setDarkMode(!darkMode)}
                                    className={`p-3 rounded-full ${darkMode ? 'bg-gray-700 hover:bg-gray-600' : 'bg-gray-100 hover:bg-gray-200'} transition-colors`}
                                    aria-label="Toggle dark mode"
                                >
                                    {darkMode ? <Sun className="w-6 h-6 text-yellow-400" /> : <Moon className="w-6 h-6 text-gray-600" />}
                                </button>
                          
