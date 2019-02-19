Installing magento 2.3
=====================================

Markup : * use incognito browser to avoid {{menuState.url}} error
         * After installation, a brown blank page will probably display on the admin page --
run on terminal or cmd (php .\bin\magento setup:static-content:deploy -f).
         *Open magento\lib\internal\Magento\Framework\View\Element\Template\File\Validator.php then replace 
public function isValid($filename)
{
    $filename = str_replace('\\', '/', $filename);
    if (!isset($this->_templatesValidationResults[$filename])) {
        $this->_templatesValidationResults[$filename] =
            ($this->isPathInDirectories($filename, $this->_compiledDir)
                || $this->isPathInDirectories($filename, $this->moduleDirs)
                || $this->isPathInDirectories($filename, $this->_themesDir)
                || $this->_isAllowSymlinks)
            && $this->getRootDirectory()->isFile($this->getRootDirectory()->getRelativePath($filename));
    }
    return $this->_templatesValidationResults[$filename];
}
with
 
public function isValid($filename)
    {
       return true;
    }
         * Installing sample data procedure
            * composer config repositories.magento composer https://repo.magento.com/packages.json
            * composer update ( Need to input your mage market username and password)
            * magento sampledata:deploy
            * magento setup:upgrade
         * Magento2 admin menu panel doesn't work
            *Got to app/etc/di.xml and find the line 
    Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink
    and replace with Magento\Framework\App\View\Asset\MaterializationStrategy\Copy
         * Magento Logo not loading
            * Open cmd or terminal and run php bin/magento setup:static-content:deploy in the magento directory.
         * Cannot replace Luma logo, unable to upload logo
            * Got to app/code/Magento/Theme/view/adminhtml/ui_component/design_config_form.xml
            * Relace <field name="head_shortcut_icon" formElement="fileUploader"> with <field name="head_shortcut_icon" formElement="imageUploader">