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
                            </div>
                            <p className={`${darkMode ? 'text-gray-300' : 'text-gray-600'}`}>S3 - السنة الثانية هندسة الدولة كهروتقنية</p>
                        </div>

                        {/* GPA Display */}
                        <div className="bg-gradient-to-r from-indigo-600 to-blue-600 rounded-lg shadow-lg p-8 mb-6 text-white">
                            <div className="flex items-center justify-between">
                                <div>
                                    <div className="flex items-center gap-2 mb-2">
                                        <TrendingUp className="w-6 h-6" />
                                        <h2 className="text-xl font-semibold">المعدل العام</h2>
                                    </div>
                                    <p className="text-5xl font-bold">{gpa}<span className="text-2xl">/20</span></p>
                                    <p className="text-lg mt-2 opacity-90">{getGPAMention(gpa)}</p>
                                </div>
                                <BookOpen className="w-24 h-24 opacity-20" />
                            </div>
                        </div>

                        {/* Subjects Grid */}
                        <div className="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
                            {subjects.map(subject => {
                                const avg = calculateSubjectAverage(subject);
                                return (
                                    <div key={subject.id} className={`${darkMode ? 'bg-gray-800' : 'bg-white'} rounded-lg shadow-md p-5 hover:shadow-lg transition-shadow`}>
                                        <div className="mb-4">
                                            <h3 className={`font-bold ${darkMode ? 'text-white' : 'text-gray-800'} text-lg mb-1`}>{subject.name}</h3>
                                            <div className="flex justify-between items-center">
                                                <span className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-500'}`}>المعامل: {subject.coef}</span>
                                                <span className={`text-sm font-semibold ${avg >= 10 ? 'text-green-600' : 'text-red-600'}`}>
                                                    المعدل: {avg.toFixed(2)}/20
                                                </span>
                                            </div>
                                        </div>
                                        
                                        <div className="space-y-3">
                                            {subject.fields.map(field => (
                                                <div key={field}>
                                                    <label className={`block text-sm font-medium ${darkMode ? 'text-gray-300' : 'text-gray-700'} mb-1`}>
                                                        {getFieldLabel(field)} ({(subject.weights[field] * 100).toFixed(0)}%)
                                                    </label>
                                                    <input
                                                        type="number"
                                                        min="0"
                                                        max="20"
                                                        step="0.25"
                                                        value={grades[subject.id][field]}
                                                        onChange={(e) => handleInputChange(subject.id, field, e.target.value)}
                                                        className={`w-full px-3 py-2 border ${darkMode ? 'bg-gray-700 border-gray-600 text-white' : 'bg-white border-gray-300 text-gray-900'} rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none`}
                                                        placeholder="0.00"
                                                    />
                                                </div>
                                            ))}
                                        </div>
                                    </div>
                                );
                            })}
                        </div>

                        {/* Details Table */}
                        <div className={`${darkMode ? 'bg-gray-800' : 'bg-white'} rounded-lg shadow-lg p-6 mt-6 overflow-x-auto`}>
                            <h3 className={`text-xl font-bold ${darkMode ? 'text-white' : 'text-gray-800'} mb-4`}>تفاصيل الحساب</h3>
                            <table className="w-full text-sm">
                                <thead className={darkMode ? 'bg-gray-700' : 'bg-gray-50'}>
                                    <tr>
                                        <th className={`px-4 py-3 text-right font-semibold ${darkMode ? 'text-gray-200' : 'text-gray-700'}`}>المادة</th>
                                        <th className={`px-4 py-3 text-center font-semibold ${darkMode ? 'text-gray-200' : 'text-gray-700'}`}>المعدل</th>
                                        <th className={`px-4 py-3 text-center font-semibold ${darkMode ? 'text-gray-200' : 'text-gray-700'}`}>المعامل</th>
                                        <th className={`px-4 py-3 text-center font-semibold ${darkMode ? 'text-gray-200' : 'text-gray-700'}`}>النقاط</th>
                                    </tr>
                                </thead>
                                <tbody className={`divide-y ${darkMode ? 'divide-gray-700' : 'divide-gray-200'}`}>
                                    {subjects.map(subject => {
                                        const avg = calculateSubjectAverage(subject);
                                        const points = avg * subject.coef;
                                        return (
                                            <tr key={subject.id} className={darkMode ? 'hover:bg-gray-700' : 'hover:bg-gray-50'}>
                                                <td className={`px-4 py-3 text-right ${darkMode ? 'text-gray-300' : 'text-gray-900'}`}>{subject.name}</td>
                                                <td className={`px-4 py-3 text-center font-semibold ${darkMode ? 'text-gray-200' : 'text-gray-900'}`}>{avg.toFixed(2)}</td>
                                                <td className={`px-4 py-3 text-center ${darkMode ? 'text-gray-300' : 'text-gray-900'}`}>{subject.coef}</td>
                                                <td className={`px-4 py-3 text-center ${darkMode ? 'text-gray-300' : 'text-gray-900'}`}>{points.toFixed(2)}</td>
                                            </tr>
                                        );
                                    })}
                                    <tr className={`${darkMode ? 'bg-indigo-900' : 'bg-indigo-50'} font-bold`}>
                                        <td className={`px-4 py-3 text-right ${darkMode ? 'text-white' : 'text-gray-900'}`}>المجموع</td>
                                        <td className={`px-4 py-3 text-center ${darkMode ? 'text-white' : 'text-gray-900'}`}>-</td>
                                        <td className={`px-4 py-3 text-center ${darkMode ? 'text-white' : 'text-gray-900'}`}>19</td>
                                        <td className={`px-4 py-3 text-center ${darkMode ? 'text-white' : 'text-gray-900'}`}>{(parseFloat(gpa) * 19).toFixed(2)}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>

                        {/* Footer */}
                        <div className="text-center mt-6 space-y-4">
                            <a 
                                href="https://t.me/uubma" 
                                target="_blank" 
                                rel="noopener noreferrer"
                                className="inline-flex items-center gap-2 bg-blue-500 hover:bg-blue-600 text-white font-semibold px-6 py-3 rounded-lg shadow-lg transition-all hover:scale-105"
                            >
                                <svg className="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm5.894 8.221l-1.97 9.28c-.145.658-.537.818-1.084.508l-3-2.21-1.446 1.394c-.14.18-.357.223-.548.223l.188-2.85 5.18-4.68c.223-.198-.054-.308-.346-.11l-6.4 4.03-2.76-.918c-.6-.187-.612-.6.125-.89l10.782-4.156c.5-.18.943.11.78.89z"/>
                                </svg>
                                تواصل معي على تلغرام
                            </a>
                            <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>جامعة - كلية الهندسة - تخصص كهروتقنية</p>
                        </div>
                    </div>
                </div>
            );
        };

        ReactDOM.render(<GPACalculator />, document.getElementById('root'));
    </script>
</body>
</html>
